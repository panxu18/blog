grpc的Java环境配置完全与项目进行整合，即通过gradle可以构建整个grpc项目。首先使用IDEA创建一个gradle的空项目然后修改配置文件。以下配置文件中主要进行的配置分为3步：

1. 引入protobuf gradle插件。
2. 使用protobuf gradle插件。
   1. 配置编译器protoc。
   2. 编译器需要使用的插件grpc
   3. 配置编译任务
3. 将编译生成的Java代码添加到Java的sourceSet中。

```groovy
plugins {
	id 'java'
	// 引入protobuf 插件
	id 'com.google.protobuf' version '0.8.14'
}

group = 'com.xp'
version = '0.0.1-SNAPSHOT'

def grpcVersion = '1.36.0'
def protobufVersion = '3.15.6'
def protocVersion = protobufVersion

repositories {
	mavenCentral()
}

sourceSets {
	main {
		// 将自动生成的代码添加到java sourceSets中，java是sourceSet name
		java {
			srcDir "build/generated/source/proto/main/grpc"
			srcDir "build/generated/source/proto/main/java"
		}
	}
}

protobuf {
	// 引入可用的编译器
	protoc {
        // 从仓库下载编译器，占位符只能在双引号字符串中使用
		artifact = "com.google.protobuf:protoc:${protocVersion}"
	}

	// 引入可用的编译器插件
	plugins {
		grpc {
            // 从仓库下载插件
			artifact = "io.grpc:protoc-gen-grpc-java:${grpcVersion}"
		}
	}

	// 配置编译任务
	generateProtoTasks {
		// 配置所有任务使用grpc插件
		all()*.plugins {
			grpc{}
		}
	}
}
```

