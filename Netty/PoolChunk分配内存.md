PoolChunk中有以下几个属性和内存分配有关：

1. `pageSize`: `PoolChunk`中每页的大小，下面以8KB为例。
2. `maxOrder`：伙伴算法中树的高度从0到maxOrder，下面以11为例。
3. `maxSubpageAllocs`：叶子节点的个数，1<<11。
4. `memoryMap`: 数组表示的二叉树，数组长度为1<<12。
5. `depthMap`：数组表示的二叉树，数组长度为1<<12;
6. `subpages`: 叶子节点对应的数组，可以继续切分为小于8KB的大小，所以使用`PoolSubpage`结构管理，数组长度为1<<11。

#### allocateNode

前面说到内存基于完全二叉树管理，2048个page作为叶子节点，使用4097个节点的完全二叉树可以管理所有内存的使用，每个非叶子节点记录当前子树所包含的叶子点中的未分配的内存数量（1<<(maxOrder - d)）。`allocateNode`方法就是在二叉树中查找指定大小的连续内存，每个节点都表示一块连续的内存。以2048号节点表示一个叶子节点(一个page)，1024号节点管理2048和2049两个叶子节点，以及1025、2050、2051号节点的分配为例：

1. 内存未分配时，[1024, 10], [2048, 11], [2049, 11]。
2. 分配一个page之后，[1024, 11], [2048, 12], [2049, 11]。
3. 再分配一个page，[1024, 12], [2048, 12], [2049, 12]。
4. 再分配2个page，[1025, 12], [2050, 11], [2051, 11]，直接分配两个连续的page。

```java
// 参数d表示所需内存大小为1<<(maxOrder - d)，以page为单位，
private int allocateNode(int d) {
        int id = 1;
        int initial = - (1 << d); // 00000100 -> 11111100,(id & initial) == 0 判断id是否小于1<<d，
        byte val = value(id);
        if (val > d) { // 无可用内存
            return -1;
        }
    	// 一定能找到可分配的内存，且节点id的范围一定在[1<<d, 1<<(d+1))
        while (val < d || (id & initial) == 0) { 
            id <<= 1;
            val = value(id);
            if (val > d) {
                id ^= 1;
                val = value(id);
            }
        }
        byte value = value(id);
        assert value == d && (id & initial) == 1 << d : String.format("val = %d, id & initial = %d, d = %d",
                value, id & initial, d);
        setValue(id, unusable); // 标记找到节点内存全部分配
        updateParentsAlloc(id); // 向父节点传递内存分配的信息，即祖先节点可分配内存需要减少
        return id;
    }
```

#### allocateSubpage

当申请的内存大小小于`pageSize`时，只需要一个page即分配一个叶子节点就可以了。因此使用`allocateNode`分配一个叶子节点，请求参数为`maxOrder`。分配的节点id一定在[2048, 4098)中，然后将其转化为叶子节点数组中的索引。

```java
private long allocateSubpage(int normCapacity) {
    	// subpage按照大小分组，同一组的subpage通过双链表连接，表头存放在一个设计好的数组中，根据subpage的大小可以定位到需要的表头。
        PoolSubpage<T> head = arena.findSubpagePoolHead(normCapacity);
        int d = maxOrder; 
        synchronized (head) {
            int id = allocateNode(d);
            if (id < 0) {
                return id;
            }

            final PoolSubpage<T>[] subpages = this.subpages;
            final int pageSize = this.pageSize;

            freeBytes -= pageSize;

            int subpageIdx = subpageIdx(id);
            PoolSubpage<T> subpage = subpages[subpageIdx];
            if (subpage == null) {
                subpage = new PoolSubpage<T>(head, this, id, runOffset(id), pageSize, normCapacity);
                subpages[subpageIdx] = subpage;
            } else {
                // 内存曾经分配出去了，但是后来释放了，所以只需要重新初始化
                subpage.init(head, normCapacity);
            }
            return subpage.allocate();
        }
    }
```

#### allocateRun

当申请的内存超过`pageSize`时，需要分配的多个page，通过`allocateNode`分配连续个page即可，大于`pageSize`的内存不需要使用subPage链管理，所以分配流程简单。

```java
private long allocateRun(int normCapacity) {
    int d = maxOrder - (log2(normCapacity) - pageShifts);
    int id = allocateNode(d);
    if (id < 0) {
        return id;
    }
    freeBytes -= runLength(id);
    return id;
}
```