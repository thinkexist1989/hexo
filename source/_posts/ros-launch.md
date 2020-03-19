---
title: 机器人操作系统ROS学习笔记：launch启动文件
mathjax: false
date: 2020-03-19 13:39:13
categories:
 - 技术探索
tags:
 - ROS
---

## launch文件作用

通过XML文件实现多节点的配置和启动。还可以自动启动ROS Master节点管理器（roscore），并可以实现每个节点的各种配置，为多个节点的操作提供便利。

## launch文件格式

XML是一种可扩展标记语言（EXtensible Markup Language），靠一个个元素的嵌套构成。被设计为传输和存储数据，焦点是数据的内容。

### `<launch>`标签

XML文件必须包含一个根标签，launch文件的根标签采用`<launch>`，其他内容均包含在此标签中。

```xml
<?xml version="1.0"?>
<launch>
    ...
</launch>
```

### `<node>`标签

`<node>`用来启动ROS节点，相当于在终端执行`rosrun`，其语法如下：

```xml
<node name="node-name" pkg="package-name" type="executable-name" />
```

`name`属性定义节点运行的名称，将覆盖节点中`init()`赋予节点的名称；`pkg`定义节点所在功能包名称；`type`定义节点的可执行文件名称。

还有一些额外的属性：

- `output="screen"`:将节点标准输出打印到终端屏幕，默认输出为日志文档(log);
- `respawn="true"`:复位属性，节点停止会自动重启，默认为`false`;
- `required="true"`:必要节点，当该节点终止时，launch中其他节点也终止;
- `ns="namespace"`:命名空间，为节点相对名称添加命名空间前缀;
- `args="arguments"`:节点需要的输入参数，**注意是`args`不是`arg`，`args`是node属性，`arg`是标签**。

### `<arg>`标签

`<arg>`用于定义launch文件中的局部变量，仅可以在当前的launch中使用，便于launch文件重构。其语法如下：

```xml
<arg name="arg-name>" default="arg-value" />
```

`default`属性未给`<arg>`赋值时的默认值，launch文件需要`<arg>`定义的参数时，可以使用`$(arg arg-name)`的方式：

```xml
<param name="foo" value="$(arg arg-name)" />
<node name="node" pkg="pkg" type="type" args="$(arg arg-name)" />
```

### `<param>`标签

`<param>`使运行时存储在ROS参数服务器中的参数，相当于全局变量，在launch文件执行后便加载到ROS参数服务器中。任何节点都可以通过`ros::param::get()`接口来获取paramter的值，也可以在终端通过`rosparam`指令来获取参数的值。其使用方式如下：

```xml
<param name="param-name" value="param-value" />
```

运行之后在参数服务器中便会加载一个`param-name`的参数，其值被设置为`param-value`。为了解决很多参数同时加载的问题，ROS还同时提供了`<rosparam>`标签，可以一次性加载多个参数，其使用方法如下：

```xml
<rosparam file="/path/to/package/config/params.yaml" command="load" ns="namespace" />
```

`<rosparam>`将`params.yaml`文件中存储的参数全部加载到ROS的参数服务器中，需要设置`command`属性设置为`load`，而且可以设置命名空间，设置后，参数名称前缀会变为`/namespace/`。

### `<remap>`标签

`<remap>`标签的目的是提供一种重映射机制，相当于给变量取别名。当在网上下载了一个功能包时，无法保证其接口和自己设计的接口一致，这时采用`<remap>`标签，可以不改动别人功能包接口的情况下，完成和自身功能包接口的通讯。

比如turtlebot的控制节点发布的速度指令话题是`/turtlebot/cmd_vel`，但我们自己设计的机器人订阅的速度控制话题是`myrobot/cmd_vel`，则可以在加载turtlebot速度控制节点时，利用`<remap>`标签将话题从`/turtlebot/cmd_vel`重映射到`/myrobot/cmd_vel`，便可以让自己的机器人接收速度指令了：

```xml
<remap from="/turtlebot/cmd_vel" to="/myrobot/cmd_vel" />
```

**注意：ROS中重映射方式很多，使用也很广泛，也可以直接在终端进行重映射。**

### `<include>`标签

在复杂系统中存在多个launch文件，互相之间也存在依赖。`<include>`标签可以包含其他launch文件，复用其中的内容，相当于C语言中include头文件。其使用方式如下：

```xml
<include file="/path/to/file/other.launch" />
```

若在other.launch文件中存在需要赋值的`<arg>`标签等，则也可以如下调用：

```xml
<include file="/path/to/file/other.launch">
    <arg name="other-arg-name" value="$(arg arg-name)">
    ...
</include>
```

## 其他

更多roslaunch的用法和标签元素可以通过访问[https://wiki.ros.org/roslaunch](https://wiki.ros.org/roslaunch)来学习。
