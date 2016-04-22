---
id: 164
title: 将ubuntu13.04装在U盘上（EFI mode）非liveCD方式
date: 2013-09-25T20:33:18+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=164
permalink: /tech/ubuntu13-04-usbflash/
duoshuo_thread_id:
  - 1340357450817077258
categories:
  - Tech
tags:
  - EFI
  - linux
  - liveCD
  - ubuntu
---
今天突发奇想，想要把ubuntu装到U盘上，不是那种liveCD，在liveCD上你在ubuntu上所做的修改在重启之后都会没了，而在本文章上所说的是将系统完全安装到U盘上，而拔掉U盘后电脑上不会留下任何痕迹。想要使用U盘上的ubuntu时，将U盘插上，然后进入BIOS或者EFI引导，使用U盘启动，就可以进入ubuntu中，然后所做的任何修改都会保留，就像把系统安装到本地磁盘了一样。

首先是按照普通方式安装ubuntu。当然不是用wubi安装，因为wubi会对电脑windows loader启动项做修改，然后U盘拔下之后，引导就会失效。这里使用的是U盘安装盘或者说是LiveUSB，制作完liveusb后，安装时应注意，选择自定义安装，然后将efi分区，swap分区，/boot分区，/分区都选在目标U盘上（这里之所以说到efi分区是因为我的电脑是efi引导的，所以U盘也采用efi引导）。还有很重要的一点是选择安装引导器时要选择安装在U盘上，不要安装在本地硬盘上。但是安装完后，重启会发现，U盘竟然引导不进去。

然后是下一步，使用LiveUSB和刚刚安装过的U盘都插在电脑上，引导进LiveUSB，选择try ubuntu，然后连上网，按照这个wiki上的介绍https://help.ubuntu.com/community/Boot-Repair,安装上boot repair。然后按照这个wiki上的说明https://help.ubuntu.com/community/UEFI#Converting\_Ubuntu\_into\_EFI\_mode将BIOS模式转为EFI模式。

最后，重启电脑，这时就可以发现可以从目标U盘进入ubuntu了。嗒哒！