---
title: 机器人操作系统ROS学习笔记：URDF
categories:
  - 技术探索
tags:
  - ROS
mathjax: false
date: 2020-03-20 13:38:02
---


## URDF

URDF(Unified Robot Description Format，统一机器人描述格式)是用来描述机器人模型的重要格式。URDF采用XML语言来描述机器人的模型。

## 文件格式

URDF采用一系列的嵌套标签来描述机器人的各个环节，比如`<link>`标签、`<joint>`标签、`<robot>`标签、`<gazebo>`标签等。

### `<robot>`标签

`<robot>`标签是完整机器人模型的最顶层标签，`<link>`和`<joint>`等标签都必须作为子元素被包含在其内部。其基本语法如下：

```xml
<robot name="name_of_robot">
 <link>....</link>
 <joint>...</joint>
 ...
</robot>
```

### `<link>`标签

`<link>`标签用于描述机器人刚体部分的外观和物理属性，其元素包括外观元素(visual)、碰撞元素(collision)以及惯性元素(inertial)等。而外观元素下又包括位姿(origin)、几何(geometry)以及材料(material)等；碰撞元素下和外观元素类似，但没有材料；惯性元素下包括质量(mass)、位姿(origin)以及惯性矩阵(inertia)等。其结构大致如下：

```xml
<link name="link_name">
    <visual>
        <origin xyz="0 0 0" rpy="0 0 0"/>
        <geometry>
            <mesh filename="package://meshes/base_link.stl" />
        </geometry>
        <material name="yellow">
            <color rgba="${200/255} ${200/255} ${199/255} 1.0" />
        </material>
    </visual>
    <collision>
        <origin xyz="0 0 0" rpy="0 0 0"/>
        <geometry>
            <cylinder length="0.005" radius="0.13"/>
        </geometry>
    </collision>
    <inertia>
        <mass value="5.0" />
        <origin xyz="-0.00000322 0.17137541 0.17514843" rpy="0.0 0.0 0.0"/>
        <inertia ixx="0.13669961" ixy="0.00000038" ixz="-0.00000068"
                                    iyy="0.14393633" iyz="-0.00049262"
                                                     izz="0.01130291"/>
    </inertia>
</link>
```

### `<joint>`标签

`<joint>`标签用于描述机器人关节的运动学和动力学属性，包括关节运动的位置和速度限制。机器人关节运动形式共6种类型：

| 关节类型 | 描述 |
| :--- | :--- |
| continuous | 旋转关节，可以绕单轴无限旋转，比如车轮关节 |
| revolute | 旋转关节，但有旋转的角度极限，比如机械臂关节 |
| prismatic | 滑动关节，沿某一轴线移动，带有位置极限，比如滑轨 |
| planar | 平面关节，允许在平面正交方向上平移或旋转 |
| floating | 浮动关节，允许平移、旋转运动 |
| fixed | 固定关节，不允许运动的特殊关节 |

关节的作用是连接两个`<link>`，这两个`<link>`分别被称为`<parent link>`和`<child link>`。

![joint](/images/ros_joint.png)

其结构大致如下：

```xml
<joint name="joint_1" type="revolute">
    <origin xyz="0 0 0.375" rpy="0 0 0"/>
    <parent link="${prefix}base_link"/>
    <child link="${prefix}link_1"/>
    <axis xyz="0 0 1"/>
    <limit lower="${radians(-180)}" upper="${radians(180)}" effort="126.0" velocity="${radians(435)}" />
    <dynamics damping="0.0" friction="0.0"/>
</joint>
```

关节下还可以包括一些其他属性：

- `<calibration>`：关节参考位置，用来校准关节的绝对位置；
- `<dynamic>`：用于描述关节的物理属性，例如阻尼(damping)，摩擦力(friction)等，在动力学仿真中用到；
- `<limit>`：用于描述运动的极限值，包括上下限位置(lower,upper)，速度限制(velocity)，力矩限制(effort)；
- `<mimic>`：用于描述该关节与已有关节的关系；
- `<safety_controller>`：用于描述安全控制器参数。

### `<gazebo>`标签

`<gazebo>`标签用于描述机器人在Gazebo仿真中所需要的参数，包括机器人材料属性、Gazebo插件等。该标签不是机器人模型必须部分，只在Gazebo仿真中才需要。

其结构大致如下：

```xml
<gazebo reference="link_name">
    <material>Gazebo/Orange</material>
    <turnGravityOff>true</turnGravityOff>
</gazebo>

<gazebo>
    <plugin name="gazebo_ros_control" filename="libgazebo_ros_control.so">
    <legacyModeNS>true</legacyModeNS>
    <robotNamespace>/myrobot</robotNamespace>
    <robotSimType>gazebo_ros_control/DefaultRobotHWSim</robotSimType>
</plugin>
</gazebo>
```

### `<transmission>`标签

`<transmission>`标签用于描述驱动器和关节之间的关系。可以对齿轮传动比和并联结构等进行建模。传动可以完成指令和关节力的转换，保持功率恒定。多个驱动器可以通过复杂的传动连接到多个关节。

其结构大致如下：

```xml
<transmission name="transmission_name">
    <robotNamespace>/myrobot</robotNamespace>
    <type>transmission_interface/SimpleTransmission</type>
    <joint name="joint_name">
        <hardwareInterface>hardware_interface/PositionJointInterface</hardwareInterface>
    </joint>
    <actuator name="actuator_name">
        <mechanicalReduction>1</mechanicalReduction>
    </actuator>
</transmission>
```

**注意**：`<hardwareInterface>`中定义了关节硬件接口，当在Gazebo中加载时应为EffortJointInterface。
