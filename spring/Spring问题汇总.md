Spring为什么默认单例bean

1. Spring通过反射，代理生成bean实例比较耗时。
2. 减少bean实例的数量，减少JVM需要回收的对象。
3. 通过缓存可以快速获取到bean。
4. 缺点：线程不安全。

##### Spring 事务怎么实现

1. Spring事务通过AOP实现。
2. `@Transaction`注解是`PointCut`

##### Spring IOC

1. 加载配置类，解析成BeanDefinition，保存在map中。
2. 将BeanDefinition进行初始化，如果有依赖关系递归初始化。
3. ApplicationContext是高级容器，除了具备BeanFactory的功能外，还指出访问文件资源等、事件发布通知，接口回调