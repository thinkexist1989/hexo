---
title: Ubuntu开机自动挂载硬盘
mathjax: false
date: 2020-01-20 09:11:36
categories:
 - 技术探索
tags:
 - Linux
 - 小技巧
---

平时我使用Windows和Linux作为主力双系统，额外采用一块固态硬盘作为数据盘，为了在Linux下使用方便，需要开机自动便将硬盘挂在到/home目录下。

## 步骤

### 1.查看硬盘信息

```bash
$sudo fdisk -l
```

其显示信息大致如下：

```bash
Device     Boot Start        End    Sectors  Size Id Type
/dev/sda1        2048 1000212479 1000210432  477G  7 HPFS/NTFS/exFAT
```

如果有多个设备，则会显示多个类似信息。

### 2. 创建需要挂载的目录

我希望将硬盘挂载到用户目录下，因此在用户目录下建立`SSD/`文件夹。

```bash
$sudo mkdir ~/SSD/
```

### 3. 查看磁盘分区的UUID

输入以下指令查看`/dev/sda1`的信息

```bash
$sudo blkid
```

其显示信息如下：

```bash
/dev/sda1: LABEL="SSD" UUID="C28070388070354F" TYPE="ntfs" PARTUUID="40ae352f-01"
```

其中`UUID="C28070388070354F"`便是挂载硬盘的UUID， 其文件系统格式为ntfs。

### 4. 配置开机自动挂载

在`/etc/fstab`文件中加入如下分区信息，让其开机自动挂载：

```text
UUID=C28070388070354F /home/think/SSD ntfs defaults 0 0
```

说明：其格式为 **<分区定位>** + **<挂载点位置>** + **<挂载磁盘类型>** + **<挂载参数>** + **<dump备份>** + **<磁盘检查>**，

- **分区定位**，可以为UUID或LABEL；
- **挂载点位置**，想要挂载硬盘的位置；
- **挂载磁盘类型**，文件系统类型，`auto`, `ext4`, `ntfs`；
- **挂载参数**， 通常为`defaults`，还可设置为`auto`, `ro`, `rw`；
- **dump备份**，dump工具决定何时备份，0表示忽略，1表示备份。通常为0；
- **磁盘检查**，决定文件系统检查顺序， 0表示检查，1表示最高优先权， 2表示其他所有需要被检查的设备。

### 5. 挂载硬盘

执行完之前的步骤已经可以在开机自动挂载了，若想手动挂载也很方便，只需要输入如下命令：

```bash
$sudo mount -a
```

这样便可根据`/etc/fstab`文件中的顺序挂载所有设备。
