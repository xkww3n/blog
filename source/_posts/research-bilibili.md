---
title: 对哔哩哔哩选择性拦截视频外链的研究
tags:
  - bilibili
  - 网络
id: '58'
categories:
  - - 笔记
date: 2020-03-14 11:02:00
---

最近在萌百看条目的时候发现了一些奇怪的现象：
<!-- more -->
![请输入图片描述](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-03-14%20110321.png) ？？？什么操作？ 在我点击这个链接后，发现：第一个链接（`【折v】中华铄金娘【心华】【短版摸鱼】 [1/1]`）打开后显示为404，而第二个链接（`这里`）却可以正常打开。但是在Chrome DevTools里，两个`<a>`标签的URL没有任何区别。 ![请输入图片描述](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-03-14%20110840.png) 唯一的区别，就是那个可访问的链接加上了3个属性：`nofollow`,`noreferrer`,`noopener`.这三个属性中的`noreferrer`可以使得数据包中不含`Referer`字段，做到当用户在一个**网页**上点击链接时，目标服务器却认为用户是通过**地址栏**直接输入URL访问的效果。 所以这个功能对萌百的所有外链有效，是针对萌百的封禁吗？ 用Fiddler抓个包看看。 ![请输入图片描述](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-03-14%20110840.png) ![请输入图片描述](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-03-14%20112044.png) 可以明显的看到，加了Referer字段的数据包被B站302到了404页面，而没加Referer字段的数据包则收到了正常的200响应。而且这个404明显不同于B站对于不存在的视频的处理规则——**不发送404响应头，不重写URL**。具体怎么证明自个去B站上输一个不存在的av号就明白了： ![请输入图片描述](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-03-14%20112641.png) 那么这些页面究竟发生了什么？我去萌百上搜索了一下，关键词为：`移动端用户请点击这里观看原视频（选择在新窗口中打开，否则会被识别为404）。`，搜出来了43条结果。 对搜出来的结果大致分析了下，发现这些视频**大多数或多或少包含一些在“净网”中定义为非法的内容**，比如说：

*   《骚红娘》，拦截外链+屏蔽搜索关键词，包含性暗示内容。
*   《罂粟花冠》，拦截外链，包含毒品有关内容。
*   《噬心》，删除原视频+拦截补档视频外链，包含暴力血腥内容。

就举三个例子吧。我也没空一个一个视频的分析。 那这是不是B站专门对萌百设置的拦截规则呢？不是，也不可能是。为什么不可能我懒得论证了，从技术上证明： ![请输入图片描述](https://cdn.xkww3n.life/%E6%89%B9%E6%B3%A8%202020-03-14%20115247.png) 那么，回归到我发现这一现象的视频——《中华铄金娘》上，这个视频为什么会被拦截外链？故事情节？那为什么不拦《中华烛火娘》？情节类似啊。 后来我终于找出了一个可能的理由：空耳-->Political sensitivity.

> 本曲“箭过后万事已休”一句被空耳成“**建国**后万事已休”。

…… 类似的，有一个视频，《元素周期表之歌》，我也找到了一个类似的理由：空耳-->色情：

> ……Mn锰 Fe铁 Co钴 **Ni镍 Cu铜** Zn锌 Ga镓 Ge锗……

是不是没看懂？我光看歌词也没看出来有什么问题，后来我听了一下这首歌……emmm…… `镍`的读音是`niè`，`铜`的读音是`tóng`，而`恋童`两字的读音分别是`liàn tóng`，两者音调相同，读音相似，可能是被误杀了。 其他不存在违规行为的视频，我大概看了下，有些还真的可以空耳出违规点来，有的……反正我猜不出来。