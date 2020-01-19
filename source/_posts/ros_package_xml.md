---
title: 机器人操作系统ROS学习笔记：package清单文件
date: 2019-8-6 14:00:00
categories:
 - 技术探索
tags: 
 - ROS
mathjax: false
---

# package清单文件

每一个ROS的功能包都包含一个package.xml的功能包清单文件，用于记录功能包的基本信息，包含作者信息、许可信息、依赖选项、编译标志等。

# package.xml规则

package.xml文件采用xml标记语言来编写，其中的主要信息包括一下几种：

- name: 功能包的名称；
- version: 功能包的版本；
- description: 功能包的描述；
- maintainer: 功能包的维护者信息；
- license: 许可信息，MIT，BSD，GPL等；

除此之外，package.xml中还包含了功能包所需的各种依赖项，主要包括：

- buildtool_depend: 编译工具依赖项，通常为catkin；
- build_depend: 功能包代码编译所需的依赖项，例如roscpp，rospy，geometry_msgs，message_generation等；
- run_depend: 功能包运行过程中所需的依赖项，例如roscpp，rospy，std_msgs，message_runtime等；

如果是元功能包，则还需包含一个引用标签：

```
 <export>
    <metapackage/>
 </export>
```
