##### AOP概念

1. JoinPoint：程序运行中的某个阶段点，比如方法的调用、异常的抛出等，甚至构造器、属性注入也是JoinPoint。
2. PointCut：部分JoinPoint的集合，需要织入增强功能的JoinPoint。
3. Advice：需要织入程序中功能。
4. Aspect：Advice和PointCut的结合。
5. Adivisor：Spring中一种特殊的Aspect，Advisor对应一个PointCut。

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Transactional {
}
```

##### 自动配置

SpringBoot在spring.factories中添加了

`org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration`启动Transaction的自动配置。加载流程如下：

1. `TransactionAutoConfiguration`
2. `EnableTransactionManagementConfiguration`
3. `@EnableTransactionManagement`
4. `TransactionManagementConfigurationSelector`
5. `ProxyTransactionManagementConfiguration`
6. 
   1. `BeanFactoryTransactionAttributeSourceAdvisor`会由`AnnotationAwareAspectJAutoProxyCreator`调用来为一个有PointCut的类生成代理。
   2. `TransactionAttributeSourcePointcut`用于检测类上是否有事务PointCut。
   3. `TransactionAttributeSource`用于在类上查找和事务有关的信息。
   4. `SpringTransactionAnnotationParser`判断是否有`@Transaction'注解。
   5. `TransactionInterceptor`拦截器，用于执行事务方法。

