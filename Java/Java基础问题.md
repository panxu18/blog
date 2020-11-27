##### 面向对象三大特效

1. 封装
2. 继承
3. 多态

##### 抽象类和接口的区别

1. 语法区别：
   1. 抽象类可以有构造方法。
   2. 抽象类中的静态方法可以在外部调用。
   3. 抽象类中可以有非抽象方法。
   4. 抽象类中可定义任意属性。
   5. 抽象类只能单继承。
2. 设计目的：接口用于规定有什么行为，抽象类用于代码复用。

##### 为什么要同时重写hashCode()和equals()方法

1. 考虑到一些散列集合的应用，如果hash和equal不一致，会出现找不到对象的情况。

2. HashMap查找元素的比较顺序：

   1. hashcode
   2. ==
   3. equals
   4. compareTo

##### HashMap负载因子为什么为0.75

1. HashMap桶中元素长度服从泊松分布，这个性质和负载因子无关。
2. 考虑空间和效率应该是根据实验选择。

##### 哈希冲突解决方法

1. 开放地址法
   1. 线性探测
   2. 再平方法
2. 链式地址法
3. 再哈希法
4. 公共溢出区

##### int a = 100，中100存在哪里?

1. a是成员变量，则存放在堆内存。
2. a是局部变量，则存放在局部变量表。

##### Error和Exception的区别

1. Error是系统异常栈溢出、内存溢出等，Exception用来处理程序异常。
2. Exception必须捕获，Error无法捕获。

##### ConcurrentHashMap size()实现

1. 没有竞争时使用CAS对`baseCount`计数。
2. 有竞争是使用CounterCell计数，每个线程在CounterCell数组中随机选择一个CounterCell。CounterCell是LongAdder的简单版。

##### 集合类的继承结构

1. `Collection`

   1. `List`
   2. `Queue`
   3. `Set`

2. `Map`

   1. `ConcurrentMap`

   2. `SortedMap`

   3. `AbastractMap`

   4. `LinkedHashMap`

   5. `HashMap`

   6. `Hashtable`

##### 编译期注解和运行时注解区别

