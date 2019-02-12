---
title: Linux让终端走Shadowsocks代理 
date: 2019-1-24 23:17:00
categories:
 - Software
tags: 
 - tutorials
 - practise
mathjax: false
---

# 方法1

在终端直接运行命令
```
export http_proxy=http://proxyAddress:port
```

这个办法的好处是简单直接，并且影响面很小（只对当前终端有效，退出就不行了）。

如果你用的是ss代理，在当前终端运行以下命令，那么`wget` `curl` 这类网络命令都会经过ss代理

```
export ALL_PROXY=socks5://127.0.0.1:1080
```

# 方法2

把代理服务器地址写入shell配置文件`.bashrc`

可直接在`.bashrc`添加下面内容
```
export http_proxy="http://localhost:port"
export https_proxy="http://localhost:port"
```

以使用shadowsocks代理为例，ss的代理端口为`1080`，那么应该设置为
```
export http_proxy="socks5://127.0.0.1:1080"
export https_proxy="socks5://127.0.0.1:1080"
```

或者直接设置`ALL_PROXY`

```
export ALL_PROXY=socks5://127.0.0.1:1080
```

`localhost`就是一个域名，域名默认指向`127.0.0.1`，两者是一样的。

保存后在终端执行

```
source ~/.bashrc
```

或者退出当前终端再起一个终端。 这个办法的好处是把代理服务器永久保存了，下次就可以直接用了。

或者通过设置alias简写来简化操作，每次要用的时候输入`setproxy`，不用了就`unsetproxy`。

```
alias setproxy="export ALL_PROXY=socks5://127.0.0.1:1080"
alias unsetproxy="unset ALL_PROXY"
alias ip="curl -i http://ip.cn"
```