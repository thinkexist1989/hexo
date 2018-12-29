---
title: Hexo+Github Pages搭建个人博客
date: 2017-10-17 20:32:00
categories:
 - Software
tags: 
 - tutorials
 - practise
mathjax: false
---

# 写在前面

折腾了整整一周，终于搭建好了我的个人博客。感谢互联网的发达让信息共享如此方便，让我仅仅用了不到三天就弄好了自己的独立博客。在这篇博客的最后我也会列出非常值得参考的几篇博客教程与网站供大家学习。

# 准备工作

在打造个人博客之前，需要进行以下准备工作：

* **[Github](www.github.com)账号注册**  这是为了在Github上部署你的博客，相当于申请了一个服务器，空间免费。
* **[七牛](https://www.qiniu.com/)账号注册** 由于Github上的免费空间有限，一些大图片保存在Github上会占用空间，七牛上提供免费的10G空间，可以存储一些图片视频之类文件。
* **申请一个域名**  有了独立域名，逼格才能满满~我是在腾讯云申请的域名，一年10多块钱。

有了这些，就可以开始打造自己的独立博客了，那么首先我们在电脑上安装一些软件。

# 软件安装

我是在Windows下使用Github的，所以需要安装以下软件：

* **[Node.js](https://nodejs.org/en/)** 一个基于 Chrome V8 引擎的JavaScript 运行时
* **[Git](https://git-scm.com/)** 用来部署Git

有了这两个软件，其实就已经可以开始接下来的部署Hexo了。不过由于Hexo是用Markdown语言来写博客的，为了方便起见，我推荐一个Markdown的编辑器：
* **[Typora](https://www.typora.io/)** 超级好用，界面还美观
* **[Visual Studio Code](https://code.visualstudio.com/)** 微软推出的代码编辑器，因为配置Hexo时候需要修改一些xml文件，感觉这个编辑器挺好用的

好啦，软件安装完毕，那么我们就开始配置域名吧~

# 域名配置

域名申请完之后要设置域名解析，设置如下：
* 域名的DNS要设置为：f1g1ns1.dnspod.net 和 f1g1ns2.dnspod.net

| 主机记录 | 记录类型  |         记录值          |
| :--: | :---: | :------------------: |
|  @   |   A   |    192.30.252.153    |
|  @   |   A   |    192.30.252.154    |
| www  | CNAME | githubname.github.io |

其中githubname为自己的github账号名称，这样你自己申请域名就能够解析到你的Github Pages上了。

# 配置Github

我们要配置SSH keys将本地的Git项目与远程Github建立联系
* 检查电脑上现有的SSH keys:
* 
```
$ cd ~/. ssh 检查本机SSH keys
```

如果提示：No such file or directory 说明为第一次使用Git

* 生成新的SSH key：
* 
```
$ ssh-keygen -t rsa -C "邮件地址@youremail.com"
```

之后回车，然后输密码，以后提交项目的时候就输入这个密码。

* 将本机的SSH key添加到Github上，在id_rsa.pub文件中。登陆Github->Accout Settings->SSH Public keys->add another public keys，把文件中的密钥粘贴到里面，点击add key。

* 测试一下，输入以下命令
```
$ ssh -T git@github.com
```

* 设置用户信息：

```
$ git config --global user.name "cnfeat"//用户名
$ git config --global user.email  "cnfeat@gmail.com"//填写自己的邮箱
```

* 在Github上创建一个新仓库，名字后缀固定，比如我的Github账号为thinkexist1989，那么仓库的名字就要设置为thinkexist1989.github.io.

# 搭建Hexo框架

Hexo是一个简单、快速、强大的博客发布工具，支持Markdown格式。

##安装Hexo
打开Git Bash

```
$ npm install -g hexo
```

##部署Hexo
新建一个文件夹，然后在这个文件夹中运行Git Bash

```
$ hexo init
```

Hexo随后会在这个文件夹下生成网站所需要的所有文件，之后可以本地部署并查看生成的网页

```
$ hexo g
$ hexo s -p 5000
```

然后在浏览器输入localhost:5000便可以查看网页了(因为我安了福昕PDF，占用了4000端口)，**建议使用Chrome浏览器**

# 安装NeXT主题

参考中的[NeXT主题官方网站](https://hexo.io/)中有更加详细的安装与配置说明，在这里只简单介绍一下安装步骤：

中有更加详细的安装与配置说明，在这里只简单介绍一下安装步骤：

## 下载主题

在Hexo文件夹下运行Git Bash：

```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

## 启用主题

打开Hexo根目录下的\_config.xml文件，找到`theme`字段，将其值改成`next`

```
theme:next
```

这样便配置好了NeXT主题，NeXT具有丰富的功能与样式配置，具体的可以参考[NeXT主题官方网站][NeXT主题官方网站]

# 发布博客

博客需要用Markdown语言编写，写好之后，将博客放到source->\_post文件夹下，这里就是存放所有博客文件的地方，之后执行如下命令：

```
$ hexo clean
$ hexo g -d
```

执行完上述语句之后，GIthub上便已经部署好了，但是还有一个重要的步骤：登陆Github，进入博客的仓库，新建一个文件，重命名为CNAME，内容填写为你自己的域名，比如www.yluoblog.cn。之后保存。这样在网页中输入自己的域名，才能够解析到Github Pages。


# 写在后面

从无到有，经过自己的一顿折腾，在网页中输入自己的域名可以登录到博客的那一刻，心情真的是很激动！我之后也会将我所学到一些东西在博客中分享出来！学习是一个快乐的过程，希望每个人都能从学习中找到属于自己的快乐！

# 参考

1. [如何搭建一个独立博客——简明Github Pages与Hexo教程](http://blog.csdn.net/poem_of_sunshine/article/details/29369785/)
2. [如何使用10个小时搭建出个人域名而又Geek的独立博客？](http://www.chinaz.com/web/2016/0105/491998.shtml)
3. [Hexo官方网站](https://hexo.io/)
4. [NeXT主题官方网站](http://theme-next.iissnan.com/getting-started.html)