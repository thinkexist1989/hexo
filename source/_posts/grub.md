---
title: GRUB简介与配置
date: 2019-1-24 23:17:00
categories:
 - Software
tags: 
 - tutorials
 - practise
mathjax: false
---

# 什么是GRUB

GNU GRUB（简称“GRUB”）是一个来自GNU项目的启动引导程序。GRUB 来自 GRand Unified Bootloader 的缩写。它的功能是在启动时从 BIOS 接管掌控、加载自身、加载 Linux 内核到内存，然后再把执行权交给内核。一旦内核开始掌控，GRUB 就完成了它的任务，也就不再需要了。GRUB是多启动规范的实现，它允许用户可以在计算机内同时拥有多个操作系统，并在计算机启动时选择希望运行的操作系统。GRUB可用于选择操作系统分区上的不同内核，也可用于向这些内核传递启动参数。

简单的说，如果你的电脑上需要同时安装多个操作系统，比如Windows，Ubuntu，Centos，RHEL等（各种LINUX发行版）的话，就可以利用GRUB来作为启动引导程序，来引导系统启动。

# GRUB菜单

GRUB 菜单的功能是当默认的内核不是想要的时，允许用户从已经安装的内核中选择一个进行引导。通过上下箭头键允许你选中想要的内核，敲击回车键会使用选中的内核继续引导进程。

GRUB 菜单也提供了超时机制，因此如果用户没有做任何选择，GRUB 就会在没有用户干预的情况下使用默认内核继续引导。敲击键盘上除了回车键之外的任何键会停止终端上显示的倒数计时器。立即敲击回车键会使用默认内核或者选中的内核继续引导进程。

# GRUB配置

GRUB的一个重要的特性是安装它不需依附一个操作系统，但是这种安装需要一个Linux/Windows副本。由于单独工作，GRUB实质上是一个微型系统，通过链式启动的方式，它可以启动所有安装的主流操作系统。

因此GRUB通常在Linux系统下进行配置。`grub.cfg`文件是GRUB配置文件。它由`grub-mkconfig`程序根据用户的配置使用一组主配置文件以及GRUB默认文件而生成。`/boot/grub/grub.cfg`文件在Linux安装时会初次生成，安装新内核时又会重新生成。但是如果需要手动配置GRUB，则不能修改这个文件，而是修改`/etc/default/grub`文件，这个文件的内容大致如下所示：

```
GRUB_DEFAULT=0
GRUB_HIDDEN_TIMEOUT=0
GRUB_TIMEOUT=10
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""

# Uncomment to enable BadRAM filtering, modify to suit your needs
# This works with Linux (no patch required) and with any kernel that obtains
# the memory map information from GRUB (GNU Mach, kernel of FreeBSD ...)
#GRUB_BADRAM="0x01234567,0xfefefefe,0x89abcdef,0xefefefef"

# Uncomment to disable graphical terminal (grub-pc only)
#GRUB_TERMINAL=console

# The resolution used on graphical terminal
# note that you can use only modes which your graphic card supports via VBE
# you can see them in real GRUB with the command `vbeinfo'
GRUB_GFXMODE=1366x768

# Uncomment if you don't want GRUB to pass "root=UUID=xxx" parameter to Linux
#GRUB_DISABLE_LINUX_UUID=true

# Uncomment to disable generation of recovery mode menu entries
#GRUB_DISABLE_RECOVERY="true"

# Uncomment to get a beep at grub start
#GRUB_INIT_TUNE="480 440 1"
```

在修改完`/etc/default/grub`文件之后，需要在终端运行`update-grub`来更新`/boot/grub/grub.cfg`文件，这样就完成了对GRUB的配置。

下面介绍几个本人比较常用的GRUB配置。

## 修改开机默认引导上次选择的操作系统

- 打开`/etc/default/grub`文件
```
sudo vim /etc/default/grub
```

- 修改并加入如下代码
```
GRUB_DEFAULT=saved
GRUB_SAVEDEFAULT=true
```

- 更新`/boot/grub/grub.cfg`文件
```
sudo update-grub
```

## 修改GRUB开机引导画面
GRUB最爽的就是开机引导画面的定制，可以下载各种大神制作的主题，使开机画面美轮美奂。

- 下载GRUB主题，将主题下的文件夹中的内容复制到`/boot/grub/themes`下
```
sudo cp -R /path/to/your_theme /boot/grub/themes
```

- 修改`/etc/default/grub`文件
  
    1. 将`GRUB_GFXMODE`修改为自己屏幕分辨率
    ```
    GRUB_GFXMODE=1366*768
    ```
    2. 修改或添加一行
    ```
    GRUB_THEME=/boot/grub/themes/your_theme/theme.txt
    ```
    3. 更新`/boot/grub/grub.cfg`文件
    ```
    sudo update-grub
    ```

# GRUB引导修复

当`/boot/grub/grub.conf`配置文件丢失, 或者关键配置出现错误, 或者MBR、UEFI记录的引导程序遭到破坏时, Linux主机启动后可能只会出现“grub>”的提示符，无法完成进一步的系统启动过程。

这表示你的grub2的配置文件损坏，GRUB找不到Ubuntu系统的引导项. 从而进入修复模式了(grub rescue), 也称救援模式。在救援模式下只有很少的命令可以用: set，ls，insmod，root，prefix。

| 命令 | 描述       |
|---|---|
|set   |查看环境变量 |
|ls    |查看设备     |
|insmod|加载模块     |
|root  |指定用于启动系统的分区，设置GRUB启动分区|
|prefix|设定GRUB启动路径|

## 进入GRUB救援模式后手动引导系统

- 利用`ls`命令列出所有磁盘分区，查找启动分区，一般情况下EFI启动分区大小为500M左右，Linux的`/boot`分区为200M～500M。
- 执行以下命令来手动引导系统
```
grub>set root=(hd0,msdos8) //假设启动分区为(hd0,msdos8)
grub>set prefix=(hd0,msdos8)/boot/grub
grub>insmod normal                     //启动normal启动
grub>normal
```
- 重启之后就可以进入Linux系统了，在进入系统之后，可以更新GRUB引导项来恢复GRUB引导
```
sudo update-grub
```
- 或者重新安装GRUB
```
sudo grub-install /dev/sda // /dev/sda为启动分区位置
```