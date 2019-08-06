---
title: 利用CMAKE编译OpenCV源码 
date: 2019-2-12 22:05:00
categories:
 - Software
tags: 
 - cmake
 - opencv
mathjax: false
---

# OpenCV是什么
OpenCV的中文名称是”开源计算机视觉库“（Open Source Computer Vision Library），于1999年由Intel建立，是一个基于开源发行的跨平台计算机视觉库，可以运行在Linux、Windows、Mac OS、Android、iOS、FreeBSD、OpenBSD等操作系统上。OpenCV由一系列C函数和C++类构成，轻量且高效。除了支持C/C++语言编译开发之外，还支持使用C#，Python、Ruby等语言的接口。

# 为什么要编译源码
在OpenCV的官网提供了许多编译好的Release版本的OpenCV二进制文件，但是由于很多人的需求不同，比如OpenCV官网提供的Windows版本是利用MSVC编译的，但是很多人却需要在Windows下使用MinGW编译代码，因此需要Windows下MinGW可以利用的二进制文件，由于OpenCV提供源码，因此可以利用OpenCV的源码编译出各种二进制文件，也可以修改官网的OpenCV源码并编译后为自己所用。

# 利用CMAKE编译OpenCV源码
关于什么是CMAKE可以参考我的另一篇博客[《利用CMAKE构建和管理软件项目》](http://yluo.name/2019/01/02/cmake/)。

- 安装完CMAKE之后，启动cmake-gui。
- 指定OpenCV**源码存放路径**。点击`Browse Source`按钮，在弹出的对话框中指定OpenCV源码存放路径`/path/to/opencv/sources`。
- 指定OpenCV**构建存放路径**。点击`Browse Build`按钮，在弹出的对话框中指定OpenCV构建文件存放路径，可随意设置，但不要放在源码路径下。
- 点击`Configure`按钮，进行第一次配置，之后会弹出编译器选择对话框，选择想要生成的项目文件（比如MSVC或者MinGW），可以使用默认的编译器，也可以指定编译器，比如在使用MinGW编译时，可以指定采用GCC和G++来编译OpenCV源码。确认无误点击`Finish`按钮开始第一次配置过程。
- 第一次配置完成后，会在主对话框上出现很多编译选项，勾选`Advanced`还会显示更多，这些都是默认的编译选项，可以进行修改，比如勾选`WITH_OPENGL`和`WITH_QT`选项等。设置完成后还需要进行第二次配置，再次点击`Configure`按钮，高亮的选项会变成正常（若还是有高亮选择则需要继续修改配置选项然后点击`Configure`配置）。
- 点击`Generate`生成项目文件，会在**构建存放路径**下生成对应的项目文件，比如VS的.sln解决方案文件，或者MinGW的Makefile文件，于是便可以利用对应的项目文件生成二进制文件了。