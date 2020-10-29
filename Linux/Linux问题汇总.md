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