消息序列化需要编写proto文件，定义消息的数据结构，下面是一个简单的message的定义。

```groovy
syntax = "proto3"; // proto版本号，必须定义在第一行

// 用于指定proto文件所在的包
package grpc.pcbook;
// 编译后生成的Java文件所在的包名
option java_package = "com.xp.grpc.pcbook.pb";
option java_multiple_files = true;

/**
 需要设置中配置import的默认路径 Setting -> language&framework -> proto buf
 且引入的message需要使用package 指定为同一个包
 */
import "processor_message.proto";
import "memory_message.proto";
import "storage_message.proto";
import "screen_message.proto";
import "keyboard_message.proto";
import "google/protobuf/timestamp.proto";

message Laptop {
  string id = 1;
  string brand = 2;
  string name = 3;
  // 引用类型
  CPU cpu = 4;
  Memory ram = 5;
  // 使用repeated表示数组类型
  repeated GPU gpus = 6;
  repeated Storage storages = 7;
  Screen screen = 8;
  Keyboard keyboard = 9;
  // 组合类型，只能选其中一个值进行赋值
  oneof weight {
    double weight_kg = 10;
    double weight_lb = 11;
  }
  double price_usd = 12;
  uint32 release_year = 13;
  google.protobuf.Timestamp updated_at = 14;
}

```

需要在项目中配置相关的依赖才能将message编译为Java文件，添加如下配置后使用gradle构建项目就可以在`build/generated/source/proto`中看到生成的Java文件。

```groovy
def grpcVersion = '1.36.0'
def protobufVersion = '3.15.6'
def protocVersion = protobufVersion
dependencies {
	// protobuf core & grpc
	implementation "com.google.protobuf:protobuf-java:${protobufVersion}"
	implementation "io.grpc:grpc-all:${grpcVersion}"
}
```

