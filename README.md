# 1[安装](https://kafka.apache.org/quickstart)
## 1.1 docker安装法
```bash
# install
docker pull bitnami/kafka:latest
# run 
docker run --name kafka -p 9092:9092 bitnami/kafka:latest
```
### 1.1.1电脑上的docker有问题
Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.
**解决方法： 通过更正或删除/etc/docker/daemon.json,就可以了**
### 1.1.2 docker pull 老失败，原因为国内镜像源
Docker pull拉取镜像报错“Error response from daemon: Get "https://registry-1.docker.io/v2”
```
/etc/docker/daemon.json
# 在里面添加国内镜像源
systemctl restart docker
```
## 1.2 本地安装 used！

电脑上有librdkafka源码：~/gzh/librdkafka-1.4.4
# 2 运行方法：本地+kraft
Generate a Cluster UUID

```bash
cd ~/gzh/kafka_2.12-3.7.0
$ KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
```

Format Log Directories

```bash
$ bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c config/kraft/server.properties
```

Start the Kafka Server

```bash
$ bin/kafka-server-start.sh config/kraft/server.properties
```

# 3 kafka有两种模式：[kafka raft（kraft) vs zookeeper mode](https://romanglushach.medium.com/the-evolution-of-kafka-architecture-from-zookeeper-to-kraft-f42d511ba242)
KRaft模式消除了对单独的ZooKeeper集群的需要，使得部署和管理Kafka集群更加容易。KRaft模式比ZooKeeper模式更具可扩展性，允许Kafka集群处理更多的流量和数据。KRaft模式比ZooKeeper模式更快，从而降低了延迟并提高了吞吐量
# 
```
# INSTALL librdkafka c开发库
apt install librdkafka-dev
```

~/kafka-c-getting-started

~/gzh/librdkafka-1.4.4/examples

```
./producer
./rdkafka_example
./producer_cpp
./rdkafka_example_cpp
./consumer
./rdkafka_complex_consumer_example
./kafkatest_verifiable_client
./rdkafka_complex_consumer_example_cpp
```
# 4 基于命令行的使用/教程
## 4.1 创建一个话题， 来存储消息/事件
kafka是一个分布式事件流平台，用于多机读写存和处理事件，事件也即记录或消息。
简单的事件比如交易事务、手机地理位置更新、下单、物联网传感器测量等。这些事件有序且存储在topics中。简而言之，话题等同于文件系统中的文件夹，事件就是存储在文件夹下的文件。
在写事件之前，必须创建话题：
```bash
bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```
查看topic的信息：
```bash
$ bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
Topic: quickstart-events        TopicId: NPmZHyhbR9y00wMglMH2sg PartitionCount: 1       ReplicationFactor: 1	Configs:
Topic: quickstart-events Partition: 0    Leader: 0   Replicas: 0 Isr: 0
```
## 4.2  写事件到话题
kafka客户端和kafka均衡器通过网络通讯来进行读写事件。一旦收到消息，均衡器以容错且持久化的方法存储消息，只要你想，可以永远存储。使用快捷客户端写入事件，一行对应一个事件。
```bash
$ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
>This is my first event
>This is my second event
```
CTRL-C停止
## 4.3  读事件到话题
```bash
$ bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
This is my first event
This is my second event
```
这里，topic所有消息都会被接收，因为有--from-beginning 这个options
## 4.4  使用kafka-connect导入数据成为事件流或导出数据
你可能在已有系统，比如关系数据库和传统消息系统中存了很多数据。kafka从外部数据源连续摄入数据，kafka-connect运行连接器，实现了定制化逻辑和外部系统交互。

# 5 五个核心 API：
除了用于管理和管理任务的命令行工具外，Kafka 五个核心 API：

-   [管理API](https://kafka.apache.org/documentation.html#adminapi):管理,管理和检查主题、代理和其他 Kafka 对象。
-   [生产者API](https://kafka.apache.org/documentation.html#producerapi)用于将事件流发布（写入）到一个或多个 Kafka 主题。
-   [消费者 API](https://kafka.apache.org/documentation.html#consumerapi) 用于订阅（读取）一个或多个主题并处理向其产生的事件流。
-    [流处理 API](https://kafka.apache.org/documentation/streams)用于实现流处理应用程序和微服务。它提供更高级别的函数来处理事件流，包括转换、状态操作（如聚合和连接）、窗口化、基于事件时间的处理等。从一个或多个主题读取输入以生成一个或多个主题的输出，从而有效地将输入流转换为输出流。
-   [导入/出API](https://kafka.apache.org/documentation.html#connect)用于构建和运行可重复使用的数据导入/导出连接器，这些连接器使用（读取）或生成（写入）来自外部系统和应用程序的事件流，以便与 Kafka 集成。例如，关系数据库（如 PostgreSQL）的连接器可能会捕获一组表的每次更改。然而，在实践中，您通常不需要实现自己的连接器，因为 Kafka 社区已经提供了数百个现成的连接器。
## [Java Maven依赖配置](https://kafka.apache.org/documentation/#api)
# [配置](https://kafka.apache.org/documentation/#configuration)
kafka使用**properties**格式的文件进行配置管理，即.prop的plain text文件。


~/gzh/librdkafka-1.4.4/examples/build

https://github.com/morganstanley/modern-cpp-kafka/tree/main/examples


Installing: /usr/local/lib/cmake/RdKafka/RdKafkaConfig.cmake
-- Installing: /usr/local/lib/cmake/RdKafka/RdKafkaConfigVersion.cmake
-- Installing: /usr/local/lib/cmake/RdKafka/FindLZ4.cmake
-- Installing: /usr/local/lib/cmake/RdKafka/RdKafkaTargets.cmake
-- Installing: /usr/local/lib/cmake/RdKafka/RdKafkaTargets-noconfig.cmake
-- Installing: /usr/local/share/licenses/librdkafka/LICENSES.txt
-- Installing: /usr/local/lib/pkgconfig/rdkafka.pc
-- Installing: /usr/local/lib/librdkafka.so.1
-- Set runtime path of "/usr/local/lib/librdkafka.so.1" to ""
-- Up-to-date: /usr/local/lib/librdkafka.so
-- Installing: /usr/local/include/librdkafka/rdkafka.h
-- Installing: /usr/local/include/librdkafka/rdkafka_mock.h
-- Installing: /usr/local/lib/pkgconfig/rdkafka++.pc
-- Installing: /usr/local/lib/librdkafka++.so.1
-- Set runtime path of "/usr/local/lib/librdkafka++.so.1" to ""
-- Up-to-date: /usr/local/lib/librdkafka++.so
-- Installing: /usr/local/include/librdkafka/rdkafkacpp.h


find_package(RdKafka REQUIRED)


# lz4
Installing libraries in /usr/local/lib
Installing headers in /usr/local/include
lz4 libraries installed
Installing binaries in /usr/local/bin
Installing man pages in /usr/local/share/man/man1
lz4 installation completed


`Find.cmake` 和 `Config.cmake` 文件都是CMake中用于查找和配置外部库的文件，但它们的用途和工作方式有所不同。理解这两者的区别有助于更好地管理和使用外部库。

### Find.cmake

1. **用途**：
   - `Find.cmake` 文件通常用于寻找那些没有提供自己的CMake配置文件的库。这些文件由CMake社区维护，或者是项目开发者自己编写。
   - 它们通常位于CMake模块路径中，例如`/usr/share/cmake-3.10/Modules/`或项目内部的`cmake/Modules/`目录。

2. **内容**：
   - `Find.cmake` 文件通常包含一系列的`find_path`、`find_library`、`find_program`等命令，用于查找库文件、头文件和程序。
   - 它们会设置一些变量，如`<PackageName>_FOUND`、`<PackageName>_INCLUDE_DIRS`、`<PackageName>_LIBRARIES`等，这些变量可以在项目中使用。

3. **使用**：
   - 通过`find_package`命令调用，例如`find_package(PackageName REQUIRED)`。
   - 如果找到了库，`<PackageName>_FOUND`会被设置为`TRUE`，否则为`FALSE`。

### Config.cmake

1. **用途**：
   - `Config.cmake` 文件通常是由库的开发者提供的，用于配置已经安装好的库。这些文件通常位于库的安装目录中。
   - 它们提供了库的详细信息，包括头文件路径、库文件路径、版本信息等，并且通常定义了导入目标（imported targets），使得使用库更加方便。

2. **内容**：
   - `Config.cmake` 文件通常包含`add_library`命令，用于定义导入目标。
   - 它们还会设置一些变量，如`<PackageName>_FOUND`、`<PackageName>_INCLUDE_DIRS`、`<PackageName>_LIBRARIES`等。
   - 可能还包括版本检查，确保使用的库版本符合要求。

3. **使用**：
   - 通过`find_package`命令调用，例如`find_package(PackageName REQUIRED)`。
   - 如果找到了库，`<PackageName>_FOUND`会被设置为`TRUE`，并且可以使用导入目标，例如`target_link_libraries(my_target PRIVATE PackageName::PackageName)`。

### 示例

#### Find.cmake 示例

假设有一个`FindFoo.cmake`文件，它可能看起来像这样：

```cmake
# FindFoo.cmake
find_path(FOO_INCLUDE_DIR foo.h
    HINTS /usr/include /usr/local/include
)

find_library(FOO_LIBRARY foo
    HINTS /usr/lib /usr/local/lib
)

include(FindPackageHandleStandardArgs)
find_package_handle_standard_args(Foo DEFAULT_MSG FOO_LIBRARY FOO_INCLUDE_DIR)

if(FOO_FOUND)
    set(FOO_LIBRARIES ${FOO_LIBRARY})
    set(FOO_INCLUDE_DIRS ${FOO_INCLUDE_DIR})
endif()
```

#### Config.cmake 示例

假设有一个`FooConfig.cmake`文件，它可能看起来像这样：

```cmake
# FooConfig.cmake
get_filename_component(PACKAGE_PREFIX_DIR "${CMAKE_CURRENT_LIST_DIR}/.." ABSOLUTE)

set(FOO_VERSION 1.0.0)

include("${CMAKE_CURRENT_LIST_DIR}/FooTargets.cmake")

check_required_components(Foo)

if(FOO_FOUND)
    set(FOO_INCLUDE_DIRS "${PACKAGE_PREFIX_DIR}/include")
    set(FOO_LIBRARIES Foo::Foo)
endif()
```

### 总结

- **`Find.cmake`**：用于查找没有提供CMake配置文件的库，通常由CMake社区或项目开发者维护。
- **`Config.cmake`**：由库的开发者提供，用于配置已经安装好的库，通常包含导入目标，使得使用库更加方便。

了解这两者的区别可以帮助你更好地理解和使用CMake中的库配置。


## 问题：: error: ‘Error’ is not a member of ‘RdKafka’
  153 |     RdKafka::Error *error      = NULL;
      |              ^~~~~
     库版本导致的问题，1 major版本没这个类
```
#include <rdkafkacpp.h>
改成
#include <librdkafka/rdkafkacpp.h>
```

## collect2: error: ld returned 1 exit status
make[2]: *** [CMakeFiles/rdkafka_complex_consumer_example.dir/build.make:84：rdkafka_complex_consumer_example] 错误 1
make[1]: *** [CMakeFiles/Makefile2:76：CMakeFiles/rdkafka_complex_consumer_example.dir/all] 错误 2
make: *** [Makefile:84：all] 错误 2

连接的库版本有问题
```
# target_link_libraries(${PROJECT_NAME} PUBLIC rdkafka++)
target_link_libraries(${PROJECT_NAME} PUBLIC /usr/local/lib/librdkafka++.so)
```