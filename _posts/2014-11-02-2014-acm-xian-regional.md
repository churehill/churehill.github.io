---
id: 56
title: 2014 ACM-ICPC西安区域赛比赛环境配置手记
date: 2014-11-02T09:39:38+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=56
permalink: /tech/2014-acm-xian-regional/
duoshuo_thread_id:
  - 1340357450817077250
categories:
  - Tech
tags:
  - 2014西安赛区
  - ACM
---
在把我们累得精疲力尽的ACM西安赛区结束几天后，老师要求我写一下这次西安赛区的比赛环境的管理和安全配置，我写完一份doc文档后，突然来了兴致于是便把内容粘到了blog上

* * *

配置比赛环境主要应老师要求，参照ICPC官网上的2014 Final的环境配置：<http://icpc.baylor.edu/worldfinals/programming-environment> ，和大神们以前的配置手记：<http://blog.renren.com/blog/320259828/818220328> 和<http://blog.renren.com/blog/240107793/937011946> ，以及周老师的公子说明的一些系统安全设置。

&nbsp;

各种环境软件版本以上链接都有，这里主要写一下比赛机器的管理和安全设置：

  1.  比赛用的系统应该有两个账户，一个低权限比赛队伍账户，一个高权限管理员账户用来方便技术人员管理，参赛队伍使用的这个低权限账户应该去掉sudo权限。
  2. 然后，为了方便管理，可以直接启用root账户，也可以在/etc/sudoers里面改一下配置，使管理员账户直接sudo不用输入密码。其实，这两种方式都可行，西安赛区用的是sudo不输密码的方法，但是比赛后总结我们觉得直接使用root账户的方法更加方便一些。
  3. 安装ssh-server，然后好配置管理员账户或者root的ssh key密钥，这样可以远程登陆批量控制。ssh在西安赛区里面发挥了超大的作用，由于联想系统批量同传前系统设置做的不是很好，后面很多配置都是ssh批量远程执行完成的，另外还有清空目录等等也是通过ssh批量远程完成的。还有比较重要的一点是，安装完ssh-server后一定要改一下/etc/ssh/sshd_config禁用密码登陆，不然比赛队伍间互相ssh登陆就完蛋了。只让使用密钥登陆，然后技术人员保管好管理员账户密钥。
  4. 禁用USB，将/lib/modules/内核版本/kernel/drivers/usb/storage目录下的usb-storage.ko改一下名字就可以了，最好不要删除，这样技术人员管理的时候，可以登陆管理员账户，然后将名字改回，然后就可以使用U盘就行传输文件了。
  5. 删除grub的recovery mode引导项，以免有队员进入recovery mode获取root密码，在/boot/grub/grub.cfg里面删除或者注释掉这个引导项就行了。
  6. 开启Ubuntu的防火墙，只开放22端口，让ssh连入。
  7. 禁用network-manager，使用/etc/network/interfaces配置IP。这样做的原因是尽管参赛队伍使用的低权限账户，无法sudo，无法在network-manager里面更改连接，但是..可以添加一个连接..，你总不希望有人添加一个IP，然后乱搞吧。但是，这个实现起来还真不好弄，特别是西安赛区有270+台电脑。
  8. 热身赛后，要清空/home/user、/tmp/和/var/tmp目录防止有人藏代码。清空目录正确方法是rm {\*, .\*} –rf（经复旦大神朱健维提醒）。当然了，清完/home/user目录还是要把Ubuntu桌面配置和各种图标文档放回去，我们当时的做法是热身赛前打包了一个/home/user目录，然后热身赛后清空目录，再把压缩包解压在这个目录。也有一更简单粗暴的方法，就是把/home和/tmp划在单独的分区，然后热身赛后用联想同传系统同传这些分区就好了。
  9. 另外，如果要在管理员账户的home目录下放一些脚本的话，最好把管理员的home目录 chmod o-rwx一下，让比赛账号看不到这个目录下的内容。

* * *

PS: 复旦的三位大神和周公子超神呀，给我们大大的涨了见识，膜拜。