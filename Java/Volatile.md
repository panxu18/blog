##### volatile作用

1. 可见性：所有线程都能看到共享内存的最新状态，不保证操作的原子性。
2. 防止指令重排。

##### volatile原理

1. 从Java内存模型理解，volatile要求d读操作(read、load、use)，写(assign、store、write)动作必须连续出现，从而保证读操作会先刷新工作内存，写操作会立即同步到主内存。

2. 通过内存屏障控制CPU的store buffer和invalid queue的行为来防止指令重排。

   

