##### 线程池参数

1. corePoolSize：
2. maximumPoolSize：
3. keepAliveTime:
4. unit:
5. workQueue：
   1. ArrayBlockingQueue：核心线程满了之后会将任务放到队列中，如果队列满了就新建线程直到达到maxPoolSize。
   2. LinkedBlockingQueue：任务会放到队列中，因为无界队列不会满，所以maxPoolSize不起作用。
   3. SynchronousQueue：任务会被立刻执行，如果没有可用线程就创建一个新线程直到达到maxPoolSize。
   4. PriorityBlockingQueue
6. threadFactory：
7. handler：
   1. CallerRunsPolicy：直接在调用者线程执行。
   2. AbortPolicy：丢弃任务，并返回异常。
   3. DiscardPolicy：丢弃任务不返回异常。
   4. DiscardOldestPolicy：丢弃之前的任务。

##### ThreadPool中keepAliveTime作用

1. 控制非核心线程最长空闲的时间，如果超过这个时间非核心线程会被销毁。
2. 调用`allowCoreThreadTimeOut()`设置允许核心线程空闲超时之后，`keepAliveTime`也能用于销毁空闲的核心线程。
3. 可以在运行时调用`setKeepAliveTime()`重新设置超时时间。

##### 线程池状态

1. RUNNING：能接受新任务。
2. SHUTDOWN：不接受新任务，已接收任务继续处理。
3. STOP：不接受新任务，不处理已添加任务，并且中断正在处理的任务。
4. TIDYING：所有任务已终止，执行狗子函数`terminated()`。
5. TERMINATED：`terminated()`执行完成后，线程池终止。

##### 线程状态

1. NEW：线程创建完成。
2. RUNNABLE：就绪状态，等待处理器调度。
3. BLOCKED：阻塞状态，等待`synchronized`的监视器锁。
4. WAITING：等待状态，`Object.wait()`、`Thread.join()`、`LockSupport.park()`。
5. TIMED_WAITING：带超时的等待，`Thread.sleep()`、`Object.wait(long)`、`Thread.join(long)`、`LockSupport.parkNanos(long)`、`LockSupport.parkUntil(long)`
6. TERMINATED：完成状态。