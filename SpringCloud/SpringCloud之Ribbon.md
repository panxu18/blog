#### 负载均衡

负载均衡可以改善跨计算机，集群的计算资源的工作负载分布，负载平衡旨在优化资源使用，最大化吞吐量最小化响应时间并避免任何单一资源的过载。

#### Ribbon

Ribbon是Netflix发布的开源项目，主要功能是提供客户端的软件负载均衡算法，将Netflix的中间层服务连接在一起。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。

#### Ribbon负载均衡策略

- 随机策略
- 轮训策略，默认策略。
- 重试策略，在一个配置时间段内，当选择的Server不成功，则尝试选择一个可用的Server。
- `BestAvailableRule`最低并发策略，逐个考察Server，如果Server的断路器打开，则忽略，选择并发连接最低的Server。
- `valiabilityFilteringRule`可用过滤策略，过滤掉一直失败并被标记为circuit trepped的Server，过滤掉高并发连接的Server。
- `ResponseTimeWeightedRule`响应时间加权策略 根据Server响应时间分配权重，响应时间越长，权重越低，被选择的概率也越低。
- `oneAvoidanceRule`区域加权策略，根据Server所在区域的性能和Server的可用性，轮训选择Server并判断一个Zone的运行性能是否可用，剔除不可用Zone中的所有Server。

#### Ribbon核心类

- `IClientConfig`，配置Ribbon客户端的相关属性。
- `ServerListUpdater`，动态更新服务器列表。
- `ServerList`，获取服务器列表。
- `ServerListFilter`，服务器列表过滤器。
- `IPing`， 检查服务器状态。
- `IRule`，负载均衡选择器。
- `ILoadBalancer`，负载均衡总控制器。

#### Ribbon和Fegin的区别

- 启动类使用的注解不同，Ribbon使用`@RibbonCLient`，Feign使用`@EnableFeignClients`。
- 服务的指定位置不同，Ribbon在`@RibbonClient`注解上声明，Fegin在定义抽象方法的接口中使用`@FeignClient`声明。
- 调用方式不同，Ribbon需要自己构建HTTP请求，模拟HTTP请求然后使用`RestTemplate`发送给其他服务，Feign使用代理对象的方法调用。