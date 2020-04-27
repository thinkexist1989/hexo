---
title: 利用CMake编译OpenCV
categories:
  - 技术探索
tags:
  - cmake
  - OpenCV
mathjax: false
date: 2020-04-27 09:53:05
---


最近需要自己编译OpenCV主要有两个出发点：

- 想用OpenCV调用Kinect摄像头，发现需要OpenCV支持OpenNI；
- 目前的SIFT，SURF算法都从OpenCV源码中移除，放到了opencv_contrib里面，需要一起编译才可以；
- 希望OpenCV支持C++11特性。

基于以上目的，开始利用CMake编译OpenCV。

## 系统要求

- opencv-3.4.5源码
- opencv_contrib-3.4.5源码
- openni库
- cmake-gui

## 文件夹结构

本次编译创建的文件夹结构如下：

```bash
└──opencv 根文件夹
   ├── src
   │   ├── opencv-3.4.5 解压后的opencv源码
   │   └── opencv_contrib-3.4.5 解压后的opencv_contrib源码
   └── build 创建一个build空文件夹，放置编译的文件
```

## 配置及编译

### CMake GUI初配置

1. 打开cmake-gui软件，点击`Browse Source...`，选择`opencv/src/opencv-3.4.5`文件夹，点击确定；
2. 点击`Browse Build`，选择`opencv/build`文件夹，点击确定；
3. 点击`Configure`，等待初次配置完成

### 配置CMake选项

1. **启用C++11特性**，在cmake-gui中查找`ENABLE_CXX11`，启用；
2. **配置opencv_contrib**，查找`OPENCV_EXTRA_MODULES_PATH`，设置值为`/path/to/opencv/src/opencv_contrib-3.4.5/modules`，随后查找`OPENCV_ENABLE_NONFREE`，启用；
3. **支持OPENNI**，查找`WITH_OPENNI`以及`WITH_OPENNI2`，根据需要启用；

### 生成

多点击几次`Configure`，直到没有错误提示，若提示找不到openni之类的，就是库没装好。随后点击`Generate`，便在build文件夹下生成对应的工程文件

### 编译

根据不同的工程文件采用对应的编译方式编译即可，在Linux下生成的Makefile，可以直接`make`编译，然后`make install`安装即可。
