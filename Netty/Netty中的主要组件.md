#### Channel

Channel接口是Netty对网络操作的抽象类，它包括基本IO操作如bind,connect,read,write等。比较常用的Channel接口的实现类是NioServerSocketChannel（服务端）和NioSocketChannel（客户端），这两个 Channel 可以和 BIO 编程模型中的ServerSocket以及Socket两个概念对应上。Netty 的 Channel 接口所提供的 API，大大地降低了直接使用 Socket 类的复杂性。

#### ChannelFuture

Netty是异步非阻塞的所有的IO操作都为异步的，因此无法立刻得到操作的结果，但是可以通过ChannelFuture接口的addListener方法注册一个ChannelFutureListener，当操作执行成功或失败后监听器就会自动触发并返回结果。

#### ChannelHandler 和 ChannelPipeline

ChannelHandler 是消息的具体处理器。他负责处理读写操作、客户端连接等事情。

ChannelPipeline 为 ChannelHandler 的链，提供了一个容器并定义了用于沿着链传播入站和出站事件流的 API 。当 Channel 被创建时，它会被自动地分配到它专属的 ChannelPipeline。

##### EventLoop

EventLoop的主要作用实际就是负责监听网络事件并调用时间处理器进行相关的IO操作。EventLoop和Channel的关系：Channel为Netty网络操作的抽象类，EventLoop负责处理到其上的Channel的IO事件，两者配合参与IO操作。

#### EventloopGroup

EventloopGroup类似一个线程池，包含多个EventLoop。