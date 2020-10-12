##### Spring 事务怎么实现

1. Spring事务通过AOP实现。
2. `@Transaction`注解是`PointCut`

##### Spring IOC

1. 加载配置类，解析成BeanDefinition，保存在map中。
2. 将BeanDefinition进行初始化，如果有依赖关系递归初始化。
3. ApplicationContext是高级容器，除了具备BeanFactory的功能外，还指出访问文件资源等、事件发布通知，接口回调