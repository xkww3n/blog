---
title: 使用Clash For Windows的TAP网卡代理所有网络流量
tags:
  - 某科学的上网方式
  - 网络
id: '11'
toc: true
categories:
  - - 笔记
date: 2020-02-14 02:02:00
---

今天想着试用Cubase Elements作为宿主软件挂买VOCALOID5 Editor送的VOCALOID4.5 Editor.结果发现这玩意剧毒。Steinberg的服务器似乎被屏蔽了，用来下载的Steinberg Download Manager还不遵守系统代理规则，死活不走代理。折腾了半天突然想起来Clash For Windows有一个"TAP Device"的功能可以全局代理系统流量，就去翻官方文档：
<!-- more -->
![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-02-13%20150829.png)
不得不说，官方文档对于这个功能介绍的实在是不走心。所以我只好猜。“使用的Profile中包含fake-ip设置：”这句话我开始没看懂，把它给的代码加在了Clash的配置下面，结果不好使。后面看懂了，知道是**加在`Profiles`中的，由提供商提供的配置文件后**，像这样：

```yaml
port: 7890
socks-port: 7891
# redir-port: 7892
allow-lan: false
mode: Rule
log-level: info
external-controller: 127.0.0.1:9090

Proxy:
- { name: "PROXY", type: vmess, server: example.net, port: 2233, uuid: 0000-0000-0000-0000, alterId: 2, cipher: none, tls: false }

Proxy Group:
- { name: "provider", type: select, proxies: ["PROXY"] }

Rule:
- DOMAIN,example.com,provider

dns:
   enable: true
   enhanced-mode: fake-ip
   listen: 127.0.0.1:53
   nameserver:
      - 1.2.4.8
      - 210.2.4.8
```

（在这之前还有一步，要在主页的"TAP Device"那里点下"Install"来安装TAP网卡） 改完保存，在`控制面板\网络和Internet\网络和共享中心`里可以看到一个叫做`cfw-tap`的网卡： ![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-02-13%20152548.png) 这个网卡的配置不需要动，这里要改的是真正的网卡，也就是这幅图中的“以太网”。点击它，在“属性”里找到`Internet 协议版本4（TCP/IPv4）`,双击打开它，把DNS改为`127.0.0.1`也就是本机： ![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-02-13%20153110.png) 然后电脑的**所有需要解析域名的TCP流量**就会走Clash设置的代理(不过好像不会遵守Rules指定的规则）。我的Steinberg Download Manager也可以正常下载了，Clash里也可以检测到流量： ![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-02-13%20153433.png) ![](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-02-13%20153543.png) 插几句题外话，Steinberg Download Manager这个东西真的很毒，开始下载之后基本上代理的带宽就占满了，其他啥也干不了。而且Minecraft Launcher似乎即使这么干也还是不走代理，反而更慢了…… 还有，Cubase LE,AI,Elements其实是同一个东西，Cubase Pro,Artist也是同一个东西，标题都是写在一起的。 最后，我的代理流量被这玩意用完了！！今天™才13日！！