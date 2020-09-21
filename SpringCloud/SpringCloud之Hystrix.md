#### 什么是服务雪崩效应

服务雪崩效应的是一种因服务提供者不可用导致服务调用者不可用，并将不可用逐渐放大的现象。服务雪崩过程：

1. 服务提供者不可用。
2. 重试加大请求流量。
3. 服务调用者不可用。

应对服务雪崩效应的策略：

- 服务降级，接口调用失败就调用本地的方法返回一个默认值，防止客户端一直等待。
- 服务熔断，接口在一个统计时间范围内的请求失败数量达到设定值就开启熔断。之后的请求就会进入提前定义好的一个熔断方法，返回错误信息。在设定时间之后尝试恢复。
- 服务隔离 ，隔离服务之间的相互影响。
- 服务监控：在服务发生调用时,会将每秒请求数、成功请求数等运行指标记录下来。

#### Histrix三种Command

- 同步

  ```java
  @HystrixCommand
  User getUserById(String id) {
    return rpcPassportService.getUserById(id);
  }
  ```
  
- 异步

  ```java
  @HystrixCommand
  Future<User> getUserByIdAsync(final String id) {
    return new Respoponse<User>() -> {
     return rpcPassportService.getUserById(id);
    }
  }
  ```


- 响应式

  ```java
  @HystrixCommand
  Observable<User> getUserByIdAsync(final String id) {
    return Observable.create(new OnSubscribe(Subscriber observer) -> {
      pbserver.onNext(rpcPassportService.getUserById(id));
    });
  }
  ```

#### Histrix资源隔离

##### 信号量资源隔离

使用信号量（计数器）来限制当个服务提供者的并发量，适用于内部一些较复杂的业务逻辑的访问，不涉及到网络调用，没有考虑到超时和异步化。`TryableSemaphore`通过`CommandKey`与`HystrixCommand`一一对应，当使用服务提供者是通过`CommadKey`获得`TryableSemaphore`，然`tryAcquire`方法获得信号量，如果没有获得信号量则进行服务降级。

##### 线程池资源隔离

使用Histrix的线程池`HystrixThreadPool`来调用服务提供者，`HystrixThreadPool`通过`ThreadPoolKey`与`HystrixCommand`一一对应。从而使服务提供方的故障只会影响一个线程池中的任务。