---
title: Ubuntu安装Nvidia显卡驱动
mathjax: false
date: 2020-02-02 23:54:41
categories:
 - 技术探索
tags:
 - Linux
 - Nvidia
---

通常Ubuntu默认安装的显卡驱动是Nouveau，而为了发挥显卡的最大性能，或者为了给Nvidia显卡安装cuda驱动，都需要将显卡驱动更换为Nvidia的官方驱动。有两种方法：

## 简单安装

若显卡较老的话一般Ubuntu自己的源便会提供驱动，在系统的Additional Driver中选择相应的驱动安装即可，一般写着tested的驱动都是经过测试的，比较稳定。

## 自行安装

若显卡较新的话，Ubuntu自己的源很多时候没有相应的驱动，这时候就需要去Nvidia的官网下载对应的驱动包，比如我现在的电脑显卡是Quadra P1000，我要去Nvidia的官网上下载Linux的驱动安装包，命名通常为NVIDIA-Linux-x86_64-440.44.run，将文件下载到系统中，随后便开始安装

### 1. 禁用Nouveau

在`/etc/modprobe.d/blacklist.conf`文件的最后一行加上一句话：`blacklist nouveau`，将Nouveau加入黑名单，保存后输入如下指令生效：

```bash
$sudo update-initramfs -u
```

### 2. 重启电脑进入命令行界面

重启之后，可以在登录界面或者进入系统之后，按`ctrl`+`alt`+`F1`进入命令行界面

### 3. 关闭桌面服务

在命令行界面下输入以下命令：

```bash
$sudo service lightdm stop
```

### 4. 安装Nvidia驱动

进入驱动安装包下载位置，首先赋予其执行权限：

```bash
$chmod +x NVIDIA-Linux-x86_64-440.44.run
```

随后运行并按照提示安装即可（通常用默认选项即可）

```bash
$sudo ./NVIDIA-Linux-x86_64-440.44.run
```

### 5.验证安装

安装完成后输入以下指令：

```bash
$sudo nvidia-smi
```

若列出了GPU的信息列表则表示安装成功。
