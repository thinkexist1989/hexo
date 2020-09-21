---
title: 机器人操作系统ROS学习笔记：自定义服务
date: 2019-8-6 15:00:00
categories:
 - 技术探索
tags: 
 - ROS
mathjax: false
---

## ROS中的服务

服务（Service）是ROS节点之间同步通讯的一种方式，允许客户端（Client）节点发布请求（Request），由服务端（Server）节点处理后反馈应答（Response）。

## 定义服务

ROS中的服务可以通过srv文件夹下的xxx.srv文件进行语言无关的接口定义。文件包括请求与应答两个数据域，使用“---”进行分割。

```bash
int64 a
int64 b
---
int64 sum
```

## 使用服务

首先需要在package.xml文件表明对消息生成系统的依赖，即构建依赖项message_generation和运行依赖项message_runtime：

```bash
<build_depend>rospy</build_depend>
<run_depend>rospy</run_depend>

<build_depend>message_generation</build_depend>
<run_depend>message_runtime</run_depend>
```

随后在CMakeLists.txt文件中利用find_package函数增加对message_generation的依赖：

```bash
find_package(catkin REQUIRED COMPONENTS
    ...
    message_generation
)
```

同时，利用catkin_package函数设置运行依赖：

```bash
catkin_package(
    ...
    CATKIN_DEPENDS message_runtime ...
)
```

并添加需要参与编译的srv文件：

```bash
add_service_files(
    FILES
    xxx.srv
    ...
)
```

确保CMake知道在消息文件更改后重新编译msg文件：

```bash
generate_messages(
    DEPENDENCIES
    std_msgs
)
```

随后便可在功能包目录下运行catkin_make进行编译，成功后可使用rossrv命令检验服务的定义是否和我们想的一样：

```bash
rossrv show xxx
```

还可以使用rossrv list来查看所有可用的服务，使用rossrv packages来查看所有提供了服务的包，使用rossrv package xxx来查看xxx功能包提供的服务。

注意，ROS中还有一个命令rosservice，这个命令是用来在ROS运行时和启动的服务进行交互的命令，例如：

```bash
rosservice call /spawn 8.0 8.0 0.0 'turtle2'
```
