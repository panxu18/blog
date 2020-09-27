#### P2P端口

P2P端口用于区块链节点之间互联，包括机构内部节点互联，跨机构互联等。节点之间互联需要联盟链的准入机制控制，节点间的链接必须依赖节点证书。

[P2P网络详细设计参见](https://fisco-bcos-documentation.readthedocs.io/zh_CN/release-2.0/docs/design/p2p/p2p.html)

[网络安全和准入控制参见](https://fisco-bcos-documentation.readthedocs.io/zh_CN/release-2.0/docs/design/security_control/index.html)

#### Channel端口

Channel端口用于**控制台**和**客户端SDK**链接Channel端口，互相之间要通过证书认证，只有经过认证的客户端才能向节点发起请求，通信数据也是采用SSL方式加密。

Channel端口应只监听内网IP地址，供机构内其他的应用服务器通过SDK连接，不应监听外网地址或接受公网的连接，以免发生不必要的安全的问题，也不要只监听本地地址（127.0.0.1或localhost），否则其他应用服务器将无法连接到节点上。

[AMOP协议参见](https://fisco-bcos-documentation.readthedocs.io/zh_CN/release-2.0/docs/manual/amop_protocol.html)

#### RPC端口

RPC是**客户端**与**区块链系统**交互的一套协议和接口，用户通过RPC接口可查询区块链相关信息（如块高、区块、节点连接等）和发送交易。

RPC连接没有做证书验证，且网络传输默认是明文的，安全性相对不高，建议只监听内网端口，用于监控、运营管理，状态查询等内部的工作流程上。目前监控脚本，区块链浏览器连接的是RPC端口。