---
title: Netgear R6850路由器救砖记录
tags:
  - 硬件
  - 网络
  - 路由器
id: '17'
toc: true
categories:
  - - 笔记
date: 2020-01-21 04:05:00
---

# 症状

在管理页面升级固件至V1.1.0.66后闪POWER灯，无WiFi。
<!-- more -->
# 解决方案

使用`nmrpflash`强刷固件。
1. 前往[https://nmap.org/npcap/](https://nmap.org/npcap/)下载安装Npcap驱动（如果下文步骤无法执行，尝试前往[https://www.winpcap.org/install/](https://www.winpcap.org/install/)下载WinPcap），[https://github.com/jclehner/nmrpflash/releases](https://github.com/jclehner/nmrpflash/releases)下载nmrpflash，解压；在[https://www.netgear.com/support/](https://www.netgear.com/support/)上输入Modal Name下载旧版本固件压缩包，解压得到`.img`格式的固件。
2. 拔掉WAN口的广域网网线，将电脑插在LAN口上，IP地址设置为路由器的保留IP段中的任意IP（我的是`10.0.0.x`，默认是`192.168.1.x`），子网掩码让Windows自己计算。
3. 将`.img`格式的固件文件和nmrpflash放在同一个文件夹内，**用管理员权限打开命令提示符**，切换到该文件夹，先输入以下命令探测网卡：

```bash
./nmrpflash -L
```

得到电脑的网卡列表，选择带有`本地连接`的那个记下编号（如`net1`）

4. 先输入下面命令，不要按回车：

```bash
./nmrpflash -i 网卡 -a 路由器IP -f img格式固件
```

路由器断电，再通电，指示灯会按照以下规律亮： POWER亮，LAN亮，POWER闪烁。
**当LAN亮起时（只有约一秒时间），按下回车，LAN常亮，nmrpflash开始上传固件**，当提示`Reboot your device now.`时，断电重启路由器，救砖完成。