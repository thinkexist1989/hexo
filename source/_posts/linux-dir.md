---
title: Linux目录树
mathjax: false
date: 2020-02-21 23:26:18
categories:
 - 技术探索
tags:
 - Linux
 - 目录树
---

## 目录树

在Linux下，所有文件与目录都是由根目录开始，然后一个个的分支下来，如树枝状。因此把这种目录配置方式称为**目录树（Directory Tree）**。Linux目录树主要有以下特征：

- 目录树的起始点为根目录（/）；
- 每一个目录不只能使用本地端文件系统，也可以使用网络上的文件系统。可以利用Network File System（NFS）服务器挂载特定目录；
- 每一个文件在此目录树中的文件名（包含完整路径）都是独一无二的。

## Linux目录配置

因为Linux有各式各样的版本，如果每个版本都采用不同的目录结构，则会对使用者造成很大的困扰。为了解决这个问题，提出了Filesystem Hierarchy Standard（FHS）标准，目前几乎所有的Linux版本目录配置均遵循FHS标准。

FHS标准其实就是规范了每个特定目录下应该放置什么样子的数据，这样使得不同的Linux系统保持目录架构基本一致，同时又可以根据有一些灵活配置。而使用者在不同Linux系统下切换更加容易。

## 各个目录的功能

为了能够简单明了的表示各个目录的功能，做了一个目录树的图。从根目录开始，各个目录均存放对应类型的数据。

 ![Linux Directory Tree](/images/linux_dir.svg)

多说一句，除了FHS之外，还有一个Linux Standard Base（LSB）标准可以遵循。可以使用`lsb_release -a`查询目前的Linux发行版是否支持LSB标准。
