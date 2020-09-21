#### 主要组件

1. 网关：Spring Cloud gateway支持长连接，客户端连接到网关，网关选择一个后端IM-Server进行连接。
2. IM-Server：消息服务器接收客户端通过网关的连接请求并保存用户的在线状态（用户和IM-Server的对应关系）。主要负责心跳、消息接收、发送。
3. Message-Service： 负责消息的编解码，存储、分发等。
4. Redis：Redis存储用户在线状态。
5. RocketMQ：主要用于业务解耦。

#### 

#### 

#### 



#### 

#### 



#### 





