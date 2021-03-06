##### 查看系统负载

1. top命令查看负载。
2. load average中的三个数分别代表最近1分钟、5分钟、15分钟的系统平均负载。代表有少个进程在竞争CPU(所有核心)

##### 查看端口占用

```bash
# 参数p 列出程序PID和name
netstat -ap | grep 8080
```

##### 查找当前目录下一个月以前创建的文件，并压缩到指定文件夹

```bash
find -type -f -mtime -30 | xargs tar -czf des.tar.gz
```

1. -exec 只能根据结果依次执行，每个文件查找执行以下-exec后面的命令。
2. xargs 将所有查找结果一起作为后面命令的参数，只执行一次。

##### 内核自动升级

``` bash
# 禁止内核升级
apt-mark hold linux-image-generic linux-headers-generic
# 允许内核升级
apt-mark unhold linux-image-generic linux-headers-generic
```

##### update-alternatives

update-alternatives用于在多个同功能的软件，或软件的多个不同版本间选择。

```bash
# 参数形式
# link是在/usr/bin/,/usr/local/bin/等默认PATH搜索目录
# name是在/etc/alternatives目录中的链接名
# path是真正的可执行程序的位置,可以在任何位置
# priority是优先级
update-alternatives --install /usr/bin/java java /opt/jdk/java8

```

- 