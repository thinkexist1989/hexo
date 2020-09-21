---
title: 机器人操作系统ROS学习笔记：ROS命名空间及重映射
date: 2019-8-6 15:00:00
categories:
 - 技术探索
tags: 
 - ROS
mathjax: false
---

## ROS的命名

命名是ROS中的基本概念。ROS中的节点、话题以及参数都必须有唯一的命名。例如机器人上的相机可以命名为camera，并且相机可以输出一个话题image，同时读取一个frame_rate的参数来控制发送图像的频率。一个有效的命名具备以下特点：

- 首字符为字母（[a-z|A-Z]）、波浪线（～）或左斜杠（/）。（不可以为下划线_）
- 后续字符可以是字母或数字（[0-9|a-z|A-Z]）、下划线（_）或者左斜杠（/）

当一个机器人上存在两个相机时，由于相机的功能几乎完全一样，我们没有必要为两个相机分别编写一个程序，同时，也不想让两个相机的话题都发布在image话题上，这样会导致两个相机的画面交替出现。更一般地，命名的冲突在机器人系统中非常容易发生，这是因为机器人上常常包含相同的硬件或软件来简化工作量，例如相同的左右机械臂、相同的相机以及相同的多个轮子。为了避免命名上的冲突，ROS中采用两种机制来处理这种情况：命名空间和重映射。

## 命名空间 Namespace

命名空间是计算机科学中的一个基本概念。根据UNIX路径和网络URI的规范，ROS使用斜杠（/）来分隔命名空间。ROS可以在不同的命名空间中启动同一个节点来避免命名冲突。

例如，具有两个相机的机器人可以在不同的命名空间`left`和`right`中启动两个相机的节点，最终会有两路图像流，分别为`left/image`和`right/image`。

## 重映射 Remap

命名空间避免了命名的冲突，以相机为例，左相机发布的图像流话题为`left/image`，而在ROS中存在另一个节点nodex，专门接收`image`话题，由于两个话题在不同的命名空间之下，因此无法接收到左相机的图像信息。为了解决这个问题，一种方法是在同一个命名空间下启动这个节点，即在`left`命名空间下启动节点nodex，于是节点变为`left/nodex`，此时接收的image话题也将变为`left/image`。但是在复杂的程序当中，有可能节点需要“深入”不止一个命名空间之中，十分不方便。因此ROS中还可以用重映射（remap）的方式来解决这个问题。

在ROS中，程序中任何一个用于命名的字符串都可以在运行时重映射。例如，ROS中的一个常用程序image_view将image主题实时渲染在窗口上。使用重映射，可以使image_view接收`left/image`或者`right/image`，而无需修改image_view的代码。

## 命名空间和重映射的使用

### 在命令行中使用

由于ROS的设计模式鼓励软件的重用，重命名在开发和部署ROS软件的时候非常普遍。为了简化操作，ROS在命令行启动节点时提供了标准语法来重命名。

- 将`image`话题重映射到`right/image`可以利用重映射语法来完成：
  
    ```bash
    $rosrun package image_view image:=right/image
    ```

- 把节点放置到命名空间中可以利用`__ns`命名空间重映射语法来完成：

    ```bash
    $rosrun package camera __ns:=right
    ```

- 修改节点的名字可以利用`__name`重命名语法来完成：

    ```bash
    $rosrun package camera __name:=camera2
    ```

### 在launch文件中使用

- 通过设置name参数来确定节点的名字：

    ```xml
    <node name="turtlesim_node2" pkg="turtlesim" type="turtlesim_node.py"/>
    ```

- 通过设置ns参数来确定默认命名空间：

    ```xml
    <node name="turtlesim_node" pkg="turtlesim" type="turtlesim_node.py" ns="sim1" />
    ```

- 通过remap标签设置重映射：

    ```xml
    <remap from="image" to="right/image"/>
    ```

### 在ROS程序终端中使用

可以利用环境变量设置默认命名空间：

```bash
$export ROS_NAMESPACE=default-namespace
```
