---
title: Ubuntu下Shadowsocks代理及终端使用ss
date: 2019-1-24 23:17:00
categories:
 - 技术探索
tags: 
 - Shadowsocks
 - bash
 - proxychains
 - Linux
mathjax: false
---

作为一名勤勤恳恳的科研人员，在使用Ubuntu的时候，避免不了要经常科学上网，最好用的莫过于Shadowsocks代理，在此简单介绍下如何在Ubuntu下使用Shadowsocks（以下简称ss）代理以及让终端也使用ss上网。

## 预备条件

- 一台境外VPS服务器，并且已经搭载好ss服务
- 本机安装了Ubuntu系统（其他Linux发行版操作类似）

## 安装shadowsocks-qt5

shadowsocks-qt5是ss在linux下的gui程序，在终端输入以下指令，添加ss-qt5的PPA源并更新

```bash
$ sudo add-apt-repository ppa:hzwhuang/ss-qt5
$ sudo apt-get update
```

随后安装ss-qt5

```bash
$ sudo apt-get install shadowsocks-qt5
```

之后运行ss-qt5，在里面配置相应的服务器信息后便可以测试连接是否成功。为了可以在浏览器里方便的使用ss，推荐Chrome浏览器下的插件SwitchyOmega，简单介绍以下SwitchOmega的配置。

## 安装配置SwitchyOmega

在Chrome商店或者[https://www.switchyomega.com/download/](https://www.switchyomega.com/download/)下载SwitchyOmega插件并安装到Chrome。

随后在SwitchOmega配置中新建一个情景模式proxy,协议选择socks 5, 地址127.0.0.1,端口1080（根据ss-qt5中的配置适当修改）。其实这样配置之后启动proxy模式便可以实现科学上网了，但是此时所有的上网流量都需要经过VPS，很多国内的网站，比如百度等也是需要经过代理上网，增加了上网的延时，因此可以配置PAC来实现墙内网站直接连接，墙外网站走代理的完美解决方案。

## 配置PAC

在SwitchyOmega的自动切换模式下，在`规则列表规则`前面的框打√，再将后面的情景模式设置为proxy，意思是规则列表中的内容，我们使用proxy情景模式。然后规则列表设置中：

```text
规则列表格式： AutoProxy
规则列表网址： https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt
```

输入上面的网址后请点击“立即更新情景模式”，更新成功后可以看到下面的更新时间和内容，这样设置完成 “规则列表规则” 后就不需要在切换规则中一个一个添加条件了。

## 配置终端走ss代理

终端下走代理需要proxychains这个小软件的帮助，首先安装proxychains

```bash
$ sudo apt-get install proxychains
```

安装完毕后，修改`/etc/proxychains.conf`中的内容，在最后[ProxyList]选项里填入ss的信息

```text
socks5 127.0.0.1 1080
```

这样就已经配置成功了，若想让终端的命令走ss代理的话，就在命令前加上`proxychains`即可。

验证是否配置成功，首先输入`curl ip.gs`查看当前未走代理的ip地址，之后输入`proxychains curl ip.gs`查看走代理的ip地址，若ip地址是VPS的服务器地址，则配置成功。
