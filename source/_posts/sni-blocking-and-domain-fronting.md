---
title: 从pixiv免代理直连看SNI阻断及其绕过方法——域前置
tags:
  - 某科学的上网方式
  - 网络
id: '151'
toc: true
categories:
  - - 笔记
date: 2020-05-01 18:35:01
---

最近在使用某pixiv第三方客户端时发现这个客户端可以直连pixiv。然而，众所周知，使用hosts访问被屏蔽的网站的方式很早就行不通了。所以我相当好奇，就乘着五一假期做点研究。
<!-- more -->
# pixiv受屏蔽情况

目前GFW屏蔽“违规网站”所用的方法有两种——DNS污染和TCP重置抢答。所以如果只用hosts解析到正确的IP地址，连接会被GFW抢先发送的RST包重置：

![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-05-01%20155913.png)

![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-05-01%20174547.png)

# 直连方案

那么那些可以直连的软件是如何做到直连的呢？我开启直连功能后抓了下这个软件发送的包：

![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-05-01%20164837.png)

欸这个Client Hello怎么和我之前看到的不一样呢？太短了吧这也！？（以下是访问百度的Client Hello）

![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-05-01%20165101.png)

和前面的包一对比就可以看出来，这个软件在访问pixiv时把数据包里的`Server Name`字段搞乱了（变成了`?124`），这样GFW**理论上**就无法通过这个字段来探知连接到的域名。
~~但我还是不明白，GFW的IP动态路由投毒大法（又称IP黑洞）是拿来干嘛用的？直接ban掉这个IP不就好了？`210.140.131.0/24`这个IP段属于日本的IDC Frontier Inc.，隶属于SoftBank。目前这个IP段似乎只有pixiv一家在使用。按理来说屏蔽了也没什么影响。~~查了一下，发现这家是个云计算服务提供商，价格死贵：[https://www.idcf.jp/english/cloud/](https://www.idcf.jp/english/cloud/)
那么为什么即使不发送有效的`Server Name`字段，依然能正常完成SSL Handshake呢？这就要谈到pixiv服务器所用的技术之一——**SNI**

# SNI

Server Name Indication，**SNI**，服务器名称指示，简单来说，是用于在一台服务器的相同端口上部署不同证书的方法，我们熟知的Cloudflare在启用CDN后提供的证书就是使用SNI部署的。当然，pixiv也是。 实现SNI的重要过程之一就是在客户端与服务器建立连接的时候就发送要连接的域名，以便服务器可以返回一张颁发给指定域名的证书。
**当用户请求了一个不存在的域名后，服务器会发送一张默认证书给客户端以便连接能够继续。**有趣的是，pixiv服务器设置的默认证书正好是**颁发给`*.pixiv.net`的泛域名证书**。所以无论请求什么东西，返回来的证书总是能用的。
下图是pixiv返回的默认证书，使用者是`*.pixiv.net`：

![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-05-01%20172533.png)

这张则是正常访问主页时，使用SNI技术签发的证书，使用者是`www.pixiv.net`：

![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-05-01%20172845.png)

事实上，GFW目前检测某个数据包是否“违规”的手段就是基于深度数据包检测的SNI阻断，即通过Server Name字段来检查域名，判断是否阻止对这个IP的连接。而这种通过混淆Server Name来**绕过**（不是对抗）阻断的方法也已经不是什么新鲜技术，这叫做**域前置**，Domain fronting. 在[维基百科](https://zh.wikipedia.org/wiki/%E5%9F%9F%E5%89%8D%E7%BD%AE)上对这一技术有介绍。
这种方法的最大局限性在于其实用性取决于服务器如何响应无效的Server Name.如果pixiv服务器在收到这样的数据包后没有返回这个泛域名证书而是拒绝连接，那这种方法显然会失效。

# 题外话

前期的审查还不算严格，但是到了现在，已经不能用“严格”来形容了，完全到了**变态**的地步。举个例子，B站手机APP的启动图曾一度长这样
（此图片来自[萌娘共享](https://commons.moegirl.org/)，采用[知识共享(Creative Commons) 署名-非商业性使用-相同方式共享 3.0 协议](https://commons.moegirl.org/%E8%90%8C%E5%A8%98%E5%85%B1%E4%BA%AB:%E7%89%88%E6%9D%83%E4%BF%A1%E6%81%AF)授权）

由于授权协议冲突，现停止从萌娘共享直接引用图片，请点击下面链接查看： [https://zh.moegirl.org.cn/File:B站原起始页面.png](https://zh.moegirl.org.cn/File:B站原起始页面.png)

但后来就变成了这样（此图片来自[萌娘共享](https://commons.moegirl.org/)，采用[知识共享(Creative Commons) 署名-非商业性使用-相同方式共享 3.0 协议](https://commons.moegirl.org/%E8%90%8C%E5%A8%98%E5%85%B1%E4%BA%AB:%E7%89%88%E6%9D%83%E4%BF%A1%E6%81%AF)授权）

由于授权协议冲突，现停止从萌娘共享直接引用图片，请点击下面链接查看： [https://zh.moegirl.org.cn/File:B站新起始页面.png](https://zh.moegirl.org.cn/File:B站新起始页面.png)

> B站对内容审查已不是一天两天，但这次竟把代表了“哔哩哔哩干杯”这句口号，代表了众多还喜欢这个网站的用户心中情怀的两杯人畜无害的啤酒都抹消掉；结合前文所述的被央视钦点和下架番剧自审等风波，可见国内网站审查环境之严峻、生存空间之狭窄。（摘自[萌娘百科](https://zh.moegirl.org/Bilibili/%E4%BA%89%E8%AE%AE%E5%92%8C%E5%BD%B1%E5%93%8D#%E5%95%A4%E9%85%92%E5%8F%98%E5%B0%8F%E7%94%B5%E8%A7%86)）

最后，放两个视频，自己体会。

<iframe src="https://player.bilibili.com/player.html?aid=2033095&bvid=BV12x411A7XQ&cid=3145513&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="500"> </iframe>

</br>

<iframe src="https://player.bilibili.com/player.html?aid=4242132&bvid=BV1us411z7QT&cid=6857205&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="500"> </iframe>