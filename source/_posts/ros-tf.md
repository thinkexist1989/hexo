---
title: 机器人操作系统ROS学习笔记：TF坐标变换
categories:
  - 技术探索
tags:
  - ROS
mathjax: false
date: 2020-03-20 15:39:19
---


## TF坐标变换

机器人中存在大量组件，各个组件之间都使用各自的相对坐标系，这些坐标系之间的变换往往十分复杂。ROS中为了方便维护这些坐标系之间的变换，提供了TF功能包。从图中也可以看出，PR2机器人上存在的坐标系十分繁杂。

![ros_pr2_tf](/images/ros_pr2_tf.png)

## TF功能包

TF是一个让用户随时间跟踪多个坐标系的功能包，它使用树型数据结构，根据时间缓冲并维护多个坐标系之间的坐标变换关系。可以查询任意时间、任意坐标系之间坐标变换。

TF以时间为轴跟踪这些坐标系的变化，并且允许开发者请求如下类型的数据：

- 5秒之前，机器人头部坐标系相对于全局坐标系的关系？
- 机器人夹取的物体相对于机器人中心坐标系的位置？
- 机器人中心坐标系相对于全局坐标位置？

所有订阅TF消息的节点都会缓冲一份所有坐标系的变换关系数据，所以这种结构**不需要中心服务器来存储任何数据**。

使用TF功能包，需要以下步骤：

- 监听TF变换(listenTransform)

接收并缓存系统中发布的所有坐标变换数据，并从中查询所需要的坐标变换关系。

- 广播TF变换(broadcastTransform)

向系统中广播坐标系之间的坐标变换关系。系统中可能会存在多个不同部分的TF变换广播，每个广播都可以直接将坐标变换关系插入TF树中，不需要再进行同步。

## TF工具

TF功能包中提供了丰富的终端工具来帮助开发者调试和创建TF变换。

1. tf_monitor

    查看TF树中所有坐标系的**发布状态**，包括发布者、平均延迟、最大延迟等信息。

    ```bash
    $rosrun tf tf_monitor
    $tf_monitor
    ```

    查看TF树中指定坐标系之间的**发布状态**。

    ```bash
    $tf_monitor <source_frame> <target_frame>
    ```

2. tf_echo

    查看指定坐标系之间的**变换关系**。

    ```bash
    $rosrun tf tf_echo
    $tf_echo <source_frame> <target_frame>
    ```

3. static_transform_publisher

    发布两个坐标系之间的静态坐标变换，这两个坐标系不发生相对位置变化。

    ```bash
    $static_transform_publisher x y z yaw pitch roll frame_id child_frame_id period_in_ms
    $static_transform_publisher x y z qx qy qz qw frame_id child_frame_id period_in_ms
    ```

    `period_in_ms`为发布频率，单位ms。该命令还可以在launch文件中使用：

    ```xml
    <launch>
        <node name="link1_broadcaster" pkg="tf" type="static_transform_publisher" args="1 0 0 0 0 0 1 link1_parent link1 100" />
    </launch>
    ```

4. view_frames

    view_frames使可视化调试工具，可以生成pdf文件，显示TF树的信息。执行方式：

    ```bash
    $rosrun tf view_frames
    ```

    命令会生成frames.pdf文件，包含可视化的TF信息。

5. roswtf

    roswtf可以分析当前的tf配置，并尝试分析其中的问题。

    ```bash
    $roswtf
    ```

## TF消息类型

TF默认发布的话题是`/tf`，消息类型是`tf2_msgs/TFMessage`，消息结构为：

```bash
geometry_msgs/TransformStamped[] transforms
  std_msgs/Header header
    uint32 seq
    time stamp
    string frame_id
  string child_frame_id
  geometry_msgs/Transform transform
    geometry_msgs/Vector3 translation
      float64 x
      float64 y
      float64 z
    geometry_msgs/Quaternion rotation
      float64 x
      float64 y
      float64 z
      float64 w
```

消息里存储着所有的坐标系变换信息，其中的`geometry_msgs/TransformStamped`为其中坐标系变换。

## robot_state_publisher功能包

robot_state_publisher提供了将关节(joint)位置信息转换为TF消息并发布的功能，可以利用URDF文件中的信息将机器人状态发布到`/tf`话题中去。

在launch文件中的使用方法：

```xml
<launch>
    <!-- Load the urdf into the parameter server. -->
    <param name="my_robot_description" textfile="$(find mypackage)/urdf/robotmodel.xml"/>

    <node pkg="robot_state_publisher" type="robot_state_publisher" name="rob_st_pub" >
        <remap from="robot_description" to="my_robot_description" />
        <remap from="joint_states" to="different_joint_states" />
    </node>
</launch>
```
