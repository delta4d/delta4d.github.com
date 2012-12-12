---
layout: post
title: "MAC安装记"
date: 2011-11-10 23:28
comments: true
categories: ["MAC"]
---

想装MAC纯属瞎折腾，现在装好了也一直扔在那里没有用。。

10月份的时候在试着装Lion，结果每次在安装进度条20%的时候就死掉了，现在装的是东皇SL v3，先说下我的配置：

DELL INSPIRON 1318

网卡 BCM5906(wired)，BCM4312(wireless)

显卡 GeForce 8400M GS

声卡 Sigmatel@Intel 82801(Ich8)

其实装MAC最重要的是看有没有对应的硬件驱动，如果都有的话就差不多了，至于其他的条件，可以到[远景论坛](http://bbs.pcbeta.com/)查看。



黑苹果的安装教程一开始看着感觉好多，现在装好了其实也没有几步，下面是在WIN下的安装教程的简单步骤，具体操作还是要自仔细看教程

1. 现在镜像，分为原版和破解版，破解版安装较为容易，原版以后可以试试，不过看教程都差不太多

2. 安装变色龙引导或bootthink

3. 将镜像dump到磁盘分区或U盘（工具为Leopard hd install helper），也可以刻盘。如何分区什么的就不说了

4. 添加对应kext（内核扩展），通常用于设备驱动程序，放在根目录Extra/Extensions下，一开始只放必要的几个就可以了

5. 不断的troubleshooting直至安装成功，我重启了不下二十次，镜像也dump了很多次，这个过程很需要耐心

6. 用WINPE将C盘重新设置为活动分区

7. 将启动盘下的kext拷到MAC根目录下，接下来还是不断的重启再troubleshooting，这里的kext可能要比安装镜像的多一点

8. 装硬件驱动

[备忘]

驱动盘kext：

AppleACPIPS2Nub.kext

ApplePS2Controller.kext

FakeSMC.kext

MAC盘kext:

AppleACPIPS2Nub.kext

ApplePS2Controller.kext

FakeSMC.kext

NullCPUPowerManagement.kext

硬件驱动：

GeForce 8400M GS: Natit.kext

BCM5906: BCM5906MEthernet.kext

Sigmatel@Intel 82801: VoodooHDA.kext
