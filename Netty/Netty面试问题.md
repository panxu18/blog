##### 线程模型

1. 使用Reactor模型基于多路复用器接受并处理用户请求，内部使用两个线程池，其中boss线程池负责处理accept事件，接收到accept事件时，将对应的socket封装到一个NioSocketChannel中并交给work线程池，有worker线程池负责read和write事件。
2. 单线程模型：所有IO都由一个线程完成，即多路复用、事件分发和处理都在一个Reactor线程上完成。
3. 多线程模型：一个Reactor线程负责多路复用、事件分发，数据处理由业务线程池完成。
4. 主从多线程模型：一个Reactor负责accept,然后将socket注册到子reactor线程池。