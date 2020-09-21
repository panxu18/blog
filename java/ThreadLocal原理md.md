`ThreadLocal`提供了线程共安全共享全局变量的方法，`TreadLocal`对象都是是线程私有的，与普通变量不同之处在于每个线程读取的变量是相互独立的，通过`get`和`set`方法得到的都是当前线程对应的值。

#### ThreadLocal结构

`ThreadLocal`可以通过两种方法来设计：

1. `ThreadLocal`看作一个`map`，其中`key`就是当前线程，`value`就是需要存储的对象。这种方案虽然能够为每个线程提供私有的变量但是`ThreadLocal`会在多个线程中共享，这里还需要使用线程安全的`map`来实现。
2. 每个线程包含一个`ThreadLocalMap`，以`ThreadLocal`作为`key`，`value`则是需要存储的对象。这种设计方案对`map`的操作都是线程安全的，显然要第一种方案好，JDK也是通过这种方案实现。

![线程局部变量结构图](image\线程局部变量结构图.png)

#### ThreadLocalMap结构和内存泄露

`ThreadLocalMap`是`ThreadLocal`实现中的重要组件，JDK对`ThreadLocalMap`进行了一些优化,通过`ThreadLocalMap`的结构可以看到，对`ThreadLocalMap`的优化主要是为了防止对使用不当找出的内存泄露。

```java
static class ThreadLocalMap {
        static class Entry extends WeakReference<ThreadLocal<?>> {
            Object value;

            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }
}

```

弱引用会在GC时被回收，这里`Entry`是一个对`ThreadLocal`的弱引用，通过弱引用在一定程度上解决了集合中常见的内存泄露问题。

`Entry`中`value`是强引用，这里应该是考虑到`value`作为线程的私有变量，会在线程中广泛使用，对其作弱引用没有意义。由于`Entry`对`value`的强引用存在，使用不当仍然会产生内存泄露。使用不当主要还是指将`ThreadLocal`变量定义在或者错误的将其传递给一个长生命周期的对象，这时`ThreadLocalMap`的对防止内存泄露的努力就白费了。

为了防止内存泄露我们在不在使用`ThreadLocal`之后需要主动调用`remove`方法释放对`ThreadLocalMap`对`value`的引用。



