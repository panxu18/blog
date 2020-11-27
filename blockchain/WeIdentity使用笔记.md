## WeIdentity合约部署

1. 前提条件区块链安装OK。

2. 下载部署工具，并按照文档安装

   [部署文档]: https://fintech.webank.com/developer/docs/weidentity/docs/weidentity-build-with-deploy.html

3. 修改`resources/fisco.properties`中智能合约部署的地址(5个)。
4. 保存`output/admin/`下的私钥。

## Sample配置和启动

1. 将build-tools工具`resuorces`目录下的所有文件拷贝到Sample的`resurces`目录下
2. 将build-tools生成的签名私钥拷贝到Sample的`keys/priv`目录下
3. 使用`build.sh`脚本编译,使用`start.sh`启动。
4. 访问http://127.0.0.1:6101/swagger-ui.html，查看demo。

