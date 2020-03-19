---
title: 计算机网络初探（四）：网络里的中间商—正向代理与反向代理
date: 2018-12-29 19:46:00
categories:
 - 理论学习
tags: 
 - 计算机网络
 - 正向代理
 - 反向代理
mathjax: false
---

知乎网络大神[车小胖](https://www.zhihu.com/people/chexiaopang/activities)对于**正向代理**与**反向代理**做了一个有趣的类比：

> 很久以前，老王去饭店吃饭，需要先到饭店，七荤八素点好菜，坐等饭菜上桌，然后大快朵颐，不亦乐乎。有了第三方订餐外卖平台，老王懒得动身前往饭店，老王打个电话或用APP，先选好某个饭店，再点好菜，外卖小哥会送上门来。这种模式被起名为**正向代理（Forward Proxy）**
> 由于某个品牌的饭店口碑特别好，食客络绎不绝涌入，第三方订餐电话也不绝于耳，但是限于饭店接待能力有限，无法提供及时服务，很多食客等得不耐烦了，纷纷铩羽而归，饭店老总看着煮熟的鸭子飞走了，心疼不已。痛定思痛，老总又成立了几个连锁饭店，形成一个集群，对外提供统一标准的菜品服务，电话订餐电话400-xxx-7777，当食客涌入饭店总台，总台将食客用大巴运到各个连锁店，这样食客既不需要排队，各连锁店都能高速运转起来，一举两得，老总乐开了花，并为此种运作模式起名为**反向代理（Reverse Proxy）**。反向代理在计算机世界里，由于单个服务器的处理客户端（用户）请求能力有一个极限，当用户的接入请求蜂拥而入时，会造成服务器忙不过来的局面，可以使用多个服务器来共同分担成千上万的用户请求，这些服务器提供相同的服务，对于用户来说，根本感觉不到任何差别。

## 概念

正向代理是一个位于客户端和目标服务器之间的代理服务器(中间服务器)。为了从原始服务器取得内容，客户端向代理服务器发送一个请求，并且指定目标服务器，之后代理向目标服务器转交并且将获得的内容返回给客户端。正向代理的情况下客户端必须要进行一些特别的设置才能使用。
  
 ![Forward Proxy](/images/forward_proxy.svg)

反向代理正好相反。对于客户端来说，反向代理就好像目标服务器。并且客户端不需要进行任何设置。客户端向反向代理发送请求，接着反向代理判断请求走向何处，并将请求转交给客户端，使得这些内容就好似他自己一样，一次客户端并不会感知到反向代理后面的服务，也因此不需要客户端做任何设置，只需要把反向代理服务器当成真正的服务器就好了。

![Reverse Proxy](/images/reverse_proxy.svg)

## 区别

正向代理需要你主动设置代理服务器ip或者域名进行访问，由设置的服务器ip或者域名去获取访问内容并返回；而反向代理不需要你做任何设置，直接访问服务器真实ip或者域名，但是服务器内部会自动根据访问内容进行跳转及内容返回，你不知道它最终访问的是哪些机器。

正向代理是代理客户端，为客户端收发请求，使真实客户端对服务器不可见；而反向代理是代理服务器端，为服务器收发请求，使真实服务器对客户端不可见。

正向代理和反向代理最关键的两点区别：

- 是否指定目标服务器

- 客户端是否要做设置

正向代理中，proxy和client同属一个LAN，对server透明； 反向代理中，proxy和server同属一个LAN，对client透明。 实际上proxy在两种代理中做的事都是代为收发请求和响应，不过从结构上来看正好左右互换了下，所以把前者那种代理方式叫做正向代理，后者叫做反向代理。

从用途上来区分：

- 正向代理：正向代理用途是为了在防火墙内的局域网提供访问internet的途径。另外还可以使用缓冲特性减少网络使用率
- 反向代理：反向代理的用途是将防火墙后面的服务器提供给internet用户访问。同时还可以完成诸如负载均衡等功能

从安全性来讲：

- 正向代理：正向代理允许客户端通过它访问任意网站并且隐蔽客户端自身，因此你必须采取安全措施来确保仅为经过授权的客户端提供服务
- 反向代理：对外是透明的，访问者并不知道自己访问的是代理。对访问者而言，他以为访问的就是原始服务器

## 使用场景

正向代理的典型用途是为在防火墙内的局域网客户端提供访问Internet的途径。正向代理还可以使用缓冲特性减少网络使用率。反向代理的典型用途是将 防火墙后面的服务器提供给Internet用户访问。反向代理还可以为后端的多台服务器提供负载平衡，或为后端较慢的服务器提供缓冲服务。

从上面的介绍也就可以猜出来正向代理的至少一个功能（比如科学上网），也即：

> 用户A无法访问facebook，但是能访问服务器B，而服务器B可以访问facebook。于是用户A访问服务器B，通过服务器B去访问facebook，，服务器B收到请求后，去访问facebook，facebook把响应信息返回给服务器B，服务器B再把响应信息返回给A。这样，通过代理服务器B，就实现了翻墙。

从上面的介绍也可以猜出来反向代理的至少一个功能（比如负载均衡），也即：

> 假设用户A访问 **[http://www.somesite.com/something.html](http://www.somesite.com/something.html)** ，但 **[www.somesite.com](www.somesite.com)** 上并不存在 **something.html**页面，于是接收用户请求的该服务器就偷偷从另外一台服务器上取回来，然后返回给用户，而用户并不知道 **something.html** 页面究竟位于哪台机器上。当我们请求 **[www.baidu.com](www.baidu.com)** 的时候，就像拨打10086一样，背后可能有成千上万台服务器为我们服务，但具体是哪一台，你不知道，也不需要知道，你只需要知道反向代理服务器是谁就好了，**[www.baidu.com](www.baidu.com)** 就是我们的反向代理服务器，反向代理服务器会帮我们把请求转发到真实的服务器那里去。

当然反向代理的作用还有很多，这里简单列举一下：

- 保护和隐藏原始资源服务器
- 加密和SSL加速
- 负载均衡
- 缓存静态内容
- 压缩
- 减速上传
- 安全
- 外网发布

## 总结

总之，正向代理代理的对象是客户端，反向代理代理的对象是服务端；正向代理隐藏了真实的客户端，反向代理隐藏了真实的服务端。