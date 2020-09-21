`PoolThreadCache`用户缓存线程申请的内存，Netty中内存需要从PoolArena中申请，而这些申请到的内存在使用完之后不会立刻还给PoolArena，而是将其缓存到线程局部变量中，PoolThreadCache正是是线程局部变量，其创建基于`PoolThreadLocalCache`，`PoolThreadCache`作为一个中间层又在一定程度上提高了内存的分配效率。

`PoolThreadCache`中包含几类属性：

1. Arena：初始化时与PoolThreadCache关联的Arena。
2. MemoryRegionCach：使用队列保存缓存的内存，
3. tinySubPageDirectCaches：`MemoryRegionCache`的数组，对应不同大小的内存。

#### 添加缓存

调用`ByteBuf`的`release`方法释放的时候，如果是池化的`ByteBuf`那么会将其放入`PoolThreadCache`中。

```java
boolean add(PoolArena<?> area, PoolChunk chunk, ByteBuffer nioBuffer,
            long handle, int normCapacity, SizeClass sizeClass) {
    // cache方法用于定位MemoryRegionCache
    MemoryRegionCache<?> cache = cache(area, normCapacity, sizeClass);
    if (cache == null) {
        return false;
    }
    return cache.add(chunk, nioBuffer, handle);
}

public final boolean add(PoolChunk<T> chunk, ByteBuffer nioBuffer, long handle) {
    Entry<T> entry = newEntry(chunk, nioBuffer, handle);
    //  ByteBuffer加入到队列中
    boolean queued = queue.offer(entry);
    if (!queued) {
        // If it was not possible to cache the chunk, immediately recycle the entry
        entry.recycle();
    }

    return queued;
}

```

#### 获取缓存

申请内存时先在`PoolThreadCache`中找是否有可用缓存，首先根据申请内存的大小定位到`MemoryRegionCache`，然后从队列中取出一块内存。

```java
boolean allocateTiny(PoolArena<?> area, PooledByteBuf<?> buf, int reqCapacity, int normCapacity) {
    // cacheForTiny 定位MemoryRegionCache
    return allocate(cacheForTiny(area, normCapacity), buf, reqCapacity);
}
private boolean allocate(MemoryRegionCache<?> cache, PooledByteBuf buf, int reqCapacity) {
    if (cache == null) {
        // no cache found so just return false here
        return false;
    }
    // 从队列中取出一个可用的内存
    boolean allocated = cache.allocate(buf, reqCapacity, this);
    if (++ allocations >= freeSweepAllocationThreshold) {
        allocations = 0;
        trim();
    }
    return allocated;
}
public final boolean allocate(PooledByteBuf<T> buf, int reqCapacity, PoolThreadCache threadCache) {
    Entry<T> entry = queue.poll();
    if (entry == null) {
        return false;
    }
    // 对buf进行初始化
    initBuf(entry.chunk, entry.nioBuffer, entry.handle, buf, reqCapacity, threadCache);
    entry.recycle();

    // allocations is not thread-safe which is fine as this is only called from the same thread all time.
    ++ allocations;
    return true;
}

```





