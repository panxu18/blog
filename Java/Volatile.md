##### volatile作用

1. 可见性：所有线程都能看到共享内存的最新状态，不保证操作的原子性。
2. 防止指令重排。

##### volatile原理

1. 从Java内存模型理解，volatile要求d读操作(read、load、use)，写(assign、store、write)动作必须连续出现，从而保证读操作会先刷新工作内存，写操作会立即同步到主内存。
2. 通过内存屏障控制CPU的store buffer和invalid queue的行为来防止指令重排。

##### Store buffer 和Invalid Queue

- CPU修改Cache时需要通知其他CPU失效缓存，并得到确认之后才能修改Cache，所以引入StoreBuffer，保存还未搜到确认的写操作。
- 同样的，为了提高对失效缓存信号的响应速度，引入Invalid Queue保存已经收到并确认但是没有处理的缓存失效信号。

##### volatile和synchronized的区别

1. volatile无锁，不能保证原子性，synchronized能保证原子性。
2. volatile只能修饰变量，synchronized可以修饰变量，方法和代码块。



