log4j12的日志等级为OFF、FATAL、ERROR、WARN、INFO、DEBUG、ALL，重要程度依次降低，通常只需要使用ERROR、WARN、INFO、DEBUG四种级别。输出日志级别在以下地方可以设置

```properties
# 将info级别及以上程度的日志输出到stdout、D以及E日志中
log4j.rootLogger = info,Console,D,E
```

在代码中可以指定使用想要输入日志使用哪一种级别

```java
log.info("message");
log.warn("message");
log.error();
log.debug();
```

在输出位置中也可以定义允许输出日志级别

```properties
log4j.appender.Console.Threshold = INFO
```



日志输出位置可以定义任意多个，常用的输出位置有

- ConsoleAppender 控制台
- FileAppender 文件
- DailyRollingFileAppender 每天产生一个日志文件
- RollingFileAppender 文件大小达到指定尺寸时产生一个新文件
- WriterAppeder 将日志信息以流格式发送到指定地方

输出位置可以自定义，控制台输出日志的配置方式如下

```
#Console 
log4j.appender.Console=org.apache.log4j.ConsoleAppender 
log4j.appender.Console.layout=org.apache.log4j.PatternLayout 
log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n
log4j.appender.Console.Threshold = INFO
```



```properties

```



