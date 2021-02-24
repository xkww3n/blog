---
title: 为什么Bitwarden再好，1Password还是香
tags:
  - 安全
  - 密码
id: '21'
toc: true
categories:
  - - 评测
date: 2020-02-04 21:10:00
---

曾经写了篇[歌颂Bitwarden的文章](https://www.xkww3n.life/2019/10/02/1pass-alternative-bitwarden.html)，但就在昨天，我实在是忍不住，弃坑Bitwarden，换回了1Password.
<!-- more -->
突然想到，其实这个标题中的"Bitwarden"和"1Password"也分别可以替换成"AMD"和"intel".表达效果差不多。
好了，接下来说说Bitwarden的缺点。
在篇文章里我说过，Bitwarden的服务器位于Azure（不是世纪互联运营的那个），1Password的服务器位于AWS.但是......Bitwarden的服务器连接速度实在是太慢太慢了。每当我想在浏览器自动填充时更新密码条目所关联的网页，都要在保存时卡好久。尴尬的是，安卓的自动填充框架在唤起自动填充服务的提供APP时，不会新开一个Activity去处理，而是直接在需要填充的Activity中处理。所以一旦遇到网络环境差，Bitwarden没法保存，我要么等，要么完全退出浏览器。
第二个问题：我不知道是我的手机有问题还是什么其他原因，需要Bitwarden自动填充时，有时不会弹出“使用Bitwarden自动填充”的提示。而1Password却没有这个问题。虽然从这个迹象来说，这就是Bitwarden的锅，但检测密码输入环境并提示用户唤起自动填充服务提供程序不是安卓的自动填充框架该做的事吗？
第三，PC端和浏览器插件过于简陋。这点我在上次就说了，PC就是用的electron给网页版加壳，插件也是，不过不用electron，直接放网页版源码就OK.性能差不说，明明是本地客户端，却会受到服务器的影响而无法打开**本地密码库**！难道有些资源还是存在服务器里的？
**注意：以下内容已过时**
~~Bitwarden已于2020年7月6日修正了下文提到的安全与隐私问题。现在可以在GitHub和F-Droid上获得没有跟踪器的Bitwarden移动应用。
最后，安全性问题。有一回我在手机上抓包，本来没想着去抓Bitwarden的包，但后来我看列表的时候，发现Bitwarden会向Google Analytics,Google Firebase发送信息。由于我没有获取手机ROOT权限，无法解密这个包，所以我不知道这里面写了什么。但我还是去网上搜了下，结果搜到了这个：[https://www.kuketz-blog.de/bitwarden-schwaechen-bei-sicherheit-und-datenschutz](https://www.kuketz-blog.de/bitwarden-schwaechen-bei-sicherheit-und-datenschutz/)~~
如果说只是前面几个问题，我都能接受。毕竟价格差距摆在那里。但这个，嗯虽然说不是啥大问题吧，但我反正忍不了。所以，1Password真香！