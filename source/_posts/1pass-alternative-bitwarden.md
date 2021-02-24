---
title: 可能是1Password的最佳替代品——开源的密码管理器Bitwarden
tags:
  - 安全
  - 密码
id: '19'
categories:
  - - 评测
toc: true
date: 2019-10-02 18:08:00
---

1Password相信大家都不陌生，这款密码管理器曾受到过许许多多的赞誉，有人说这是“世界上最好用的密码管理器”。当然，它那高昂的售价也让很多人望而却步（比如我）： ![](https://cdn.xkww3n.life/1pass-pricing.png) 当然，网上也有许多通过它的家庭方案来降低费用的方法。但是在我看来，这种方法虽然得了便宜，但却不怎么安全，因为整个账号的管理权限是掌握在家庭管理员的手上的，这样有两个个坏处：
<!-- more -->
*   管理员可以随时删除你的账户

> If you’re a family organizer, you can stop sharing everything with a specific family member by deleting their account. After you delete a family member’s account, they cannot sign in to 1Password, which means: - **They lose access to all shared items,** including those in the Shared vault. This will not affect other family members’ access to shared items. - **They lose all the items in their Private vault.** Because the items weren’t shared with any other family members, no one will be able to access them.

*   忘记密码之后没办法自己恢复

> You can’t recover your own account, so make sure at least two family or team members can recover accounts. That way, if you can’t sign in, someone will be able to help you.

嗯，虽然对于一些人来说这不是什么大事，但这两条足以把我劝退了。所以只得去找其他的密码管理器。 我选择一个密码管理器的标准是：

1. 安全第一，曾经出现过数据泄露事件的不考虑。（LastPass说你呢）
2. 跨平台，必须要在Windows，Linux，Android设备上可以访问。 
3. 数据自主管理，如果是基于云的服务要可以导出和删除数据，本地服务需要可以导出为通用的格式。 
4. 最好是开源的。
5. 价格不能太高。
6. 
符合这些条件的密码管理器实在是少的要命啊！很长一段时间，我都在用chrome浏览器内置的密码保存功能。但是就这个都没有完全符合第3点（删数据必须要和Google账户一起删）。后来，在chrome扩展商店里闲逛，发现了这个密码管理器的chrome扩展。就去官网上看了看。

# 基本体验

![](https://cdn.xkww3n.life/20190821124034.png)
看网上的介绍，感觉不错。开源，跨平台，免费使用，还可以自建服务器。就注册了。
先来看看网页端：
![](https://cdn.xkww3n.life/20190821124706.png)
![](https://cdn.xkww3n.life/20190821124922.png)
![](https://cdn.xkww3n.life/20190821125037.png)
感觉还不错，还支持TOTP，然而这需要氪金。那就氪吧，去看看价格：
![](https://cdn.xkww3n.life/20190821125211.png)
（我这是付过了的）价格为10$/年，不算贵，比起1password来不知道好到那里去了。高级会员的功能有：

*   TOTP
*   使用YubiKey，DUO，FIDO U2F登录
*   1G加密存储空间（想多要可以继续氪金，价格为4$/GB/年）
*   公开密码报告
*   重复使用的密码报告
*   数据泄露报告

就那么些功能，看起来不多，那么还是和1Passowrd对比，有什么区别呢？

| 功能     | Bitwarden                               | 1Password                               |
|--------|-----------------------------------------|-----------------------------------------|
| TOTP   | ✓                                       | ✓                                       |
| MFA登录  | ✓                                       | ✓                                       |
| 加密存储空间 | ✓（1GB）                                  | ✓（1GB）                                  |
| 密码安全报告 | ✓                                       | ✓                                       |
| 项目     | Bitwarden                               | 1Password                               |
| 加密     | AES 256bit,PBKDF2-HMAC-SHA256,WebCrypto | AES 256bit,PBKDF2-HMAC-SHA256,WebCrypto |
| 安全审计   | 第三方开源库，安全公司，开源代码                        | 第三方开源库，社区                               |
| 服务器位置  | Microsoft Azure（可自行托管）                  | Amazon Web Services（可自行托管）              |


加密方式也没区别，1Password没有安全公司来审计可能是因为他们公司本身就是干这行的，没必要找其他人去做安全审计。服务器……两大云服务巨头没跑了，没必要比。 可以看出两者的安全性几乎是一样的，Bitwarden还开源。

# 使用体验

这个……对于Bitwarden绝对是个降分项。尤其是在PC端的使用体验。由于我的1Password账号过期没续费，所以没法继续对比。

## Windows（PC）

PC上的客户端就是个网页下载到本地再加个壳子。在Linux系统的时候忘了截图，就看看Windows的： ![](https://cdn.xkww3n.life/20190821132542.png) 体验跟浏览器端差不多。这点1Password要强于它。

## Android（移动平台）

功能上，1Password和Bitwarden没啥区别。毕竟就那么点功能，做不出花来。什么Android自动填充框架，基于辅助功能的自动填充等等等等，都有。（懒得截图了） 但是，在UI上，尤其是一些细节上，1Password的处理比Bitwarden要好。比如启动时输入PIN的时候，感觉1Password起码还知道把密码字符居中，bitwarden就没有。不过又不是不能用。

## 浏览器

两者的chrome插件我都试了下，用起来也差不多。就是Bitwarden的插件怎么那么像Android APP用HTML写出来的样子？这一点两者持平。

# 总结

总的来说，在安全性上，Bitwarden略强于1Password；价格上1Password被Bitwarden吊打；使用体验上1Password吊打Bitwarden。
如果你不差钱，对于开源啊什么的不在意就去选1Password；如果你没什么钱就无脑选Bitwarden；对开源软件有好感的也可以去用Bitwarden~~顺便帮它改改UI~~