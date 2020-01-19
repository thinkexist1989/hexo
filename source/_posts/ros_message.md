---
title: 机器人操作系统ROS学习笔记：自定义消息类型
date: 2019-8-6 15:00:00
categories:
 - 技术探索
tags: 
 - ROS
mathjax: false
---

## ROS消息文件

ROS的消息文件是一个描述ROS中所使用消息类型的简单文本，通常存放在功能包文件夹下的msg文件夹下，可以被用来生成不同语言的源代码。

## 消息数据类型

ROS中的消息可使用的基本数据类型如下：

- int8, int16, int32, int64 (plus uint*)
- float32, float64
- string
- time, duration
- 可变长度的数组，例如int8[]，以及固定长度的数组，例如float32[10]

ROS消息中还有一个特殊的数据类型：Header， 其含有时间戳和坐标系信息。通常在很多msg文件的第一行有：

```vim
Header header
```

个人理解（不一定准确），Header类型通常包含在频繁发送的消息中，这样订阅节点可以根据Header中的时间戳信息等做信息同步处理。

同时，msg文件中还可以包括其他消息文件类型，例如在下面xxx.msg文件中，使用了Header, string以及另外两个消息类型。

```vim
Header header
string child_frame_id
geometry_msgs/PoseWithCovariance pose
geometry_msgs/TwistWithCovariance twist
```

## 使用消息

为了使用消息，需要在package.xml中添加编译依赖和执行依赖：

```cmake
<build_depend>message_generation</build_depend>
<exec_depend>message_runtime</exec_depend>
```

并且在CMakeLists.txt文件中利用find_package函数增加对message_generation的依赖：

```cmake
find_package(catkin REQUIRED COMPONENTS
    ...
    message_generation
)
```

同时，利用catkin_package函数设置运行依赖：

```cmake
catkin_package(
    ...
    CATKIN_DEPENDS message_runtime ...
)
```

并添加需要参与编译的msg文件：

```cmake
add_message_files(
    FILES
    xxx.msg
    ...
)
```

确保CMake知道在消息文件更改后重新编译msg文件：

```cmake
generate_messages(
    DEPENDENCIES
    std_msgs
)
```

随后便可在功能包目录下运行catkin_make进行编译，成功后可使用rosmsg命令查看自定义的消息类型：

```bash
rosmsg show xxx
```

## 消息相关命令

- rosmsg show xxx: 查看消息xxx类型，可以不指定功能包名。
