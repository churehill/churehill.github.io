---
id: 208
title: Linux进阶：让效率翻倍的Bash技巧（一）
date: 2014-12-18T14:49:27+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=208
permalink: /tech/bash-skills-part1/
duoshuo_thread_id:
  - 1340357450817077275
categories:
  - Tech
---
许多使用过Linux一段时间的人通过一些基础操作已经能够把Linux各方面基本玩转，但是如果没有经过系统学习的话就容易缺乏一些实战技巧。这系列文章介绍一些关于bash的能够提高效率的技巧，主要是关于历史命令操作和一些快捷键，让你在命令行下工作效率翻倍，而且这些技巧不失为装逼利器呀。

#### **历史命令操作篇**

* * *

  * <span style="color: #dc5425;"> 最基本的查看历史命令 <strong>history</strong></span>

<pre class="lang:sh decode:true">history</pre>

<p style="padding-left: 30px;">
  <img class="alignnone  wp-image-209" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/bash1.png" alt="bash1" width="489" height="223" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/bash1-300x137.png 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/bash1.png 588w" sizes="(max-width: 489px) 100vw, 489px" />
</p>

  * <span style="color: #dc5425;"><strong>!n</strong> 编号为n的历史命令</span>

<p style="padding-left: 30px;">
  不用再复制粘贴，或者照着历史记录敲了。执行历史命令记录里面的某个命令，只需要 ! + 这条命令记录前的序号，比如
</p>

<pre class="lang:sh decode:true" style="padding-left: 60px;">!767</pre>

<p style="padding-left: 30px;">
  这样就可以执行767序号对应的命令 ping www.tpircsboy.com
</p>

<p style="padding-left: 30px;">
  <img class="alignnone  wp-image-210" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/bash2.jpg" alt="bash2" width="582" height="144" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/bash2-300x74.jpg 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/bash2.jpg 630w" sizes="(max-width: 582px) 100vw, 582px" />
</p>

  * <span style="color: #dc5425;"><strong>!-n</strong>  倒数第n个历史命令</span>

<p style="padding-left: 30px;">
  你也可以用  !  - （倒数第几个命令）来执行历史命令，比如 !-1 就是倒数第一个命令， !-3就是倒数第三个命令
</p>

<pre class="lang:sh decode:true ">!-3</pre>

<p style="padding-left: 30px;">
  <img class="alignnone  wp-image-211" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/7BC195C9-1FC9-4B02-A493-904A676BE9D4.jpg" alt="7BC195C9-1FC9-4B02-A493-904A676BE9D4" width="508" height="147" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/7BC195C9-1FC9-4B02-A493-904A676BE9D4-300x87.jpg 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/7BC195C9-1FC9-4B02-A493-904A676BE9D4.jpg 529w" sizes="(max-width: 508px) 100vw, 508px" />
</p>

  * <span style="color: #dc5425;"><strong>!!</strong>  上一条命令</span>

<p style="padding-left: 30px;">
  !! 表示上一条命令，相当于 !-1 。
</p>

<p style="padding-left: 30px;">
  这是一个极为方便实用的命令，比如一条很长的命令而且需要管理员权限，但是好不容易敲完但忘记加sudo，这里就可以直接用 sudo !!来完成刚刚的那条复杂的命令加sudo
</p>

<pre class="lang:default decode:true" style="padding-left: 30px;">sudo !!</pre>

<p style="padding-left: 30px;">
  <img class="alignnone  wp-image-216" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/AEB1DDBF-8F29-431D-9C8E-D129E797CBAE.jpg" alt="AEB1DDBF-8F29-431D-9C8E-D129E797CBAE" width="563" height="102" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/AEB1DDBF-8F29-431D-9C8E-D129E797CBAE-300x54.jpg 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/AEB1DDBF-8F29-431D-9C8E-D129E797CBAE.jpg 580w" sizes="(max-width: 563px) 100vw, 563px" />
</p>

  * <span style="color: #dc5425;"><strong>!keyword</strong>  查找包含该keyword的历史命令</span>

<p style="padding-left: 30px;">
  如果想查找包含某个关键字的历史命令，可以这样做
</p>

<pre class="lang:default decode:true">!keyword</pre>

<p style="padding-left: 30px;">
  查找包含keyword的历史命令，然后回车就能执行这条历史命令
</p>

<p style="padding-left: 30px;">
  <img class="alignnone size-full wp-image-220" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/DB21349F-24B3-4B14-9DF6-A96C62845DE6.jpg" alt="DB21349F-24B3-4B14-9DF6-A96C62845DE6" width="256" height="46" />
</p>

<p style="padding-left: 30px;">
  但是其实这个操作是很危险的，假如你看错或者记混了历史命令，在回车前你其实不知道要查找出的是哪条历史命令，而回车后这条命令就执行了，没有机会给你看一下查找出的命令具体是什么就执行了。很危险，不推荐这样做，可以使用 MagicSpace（见下文） 或者使用Ctrl + R 反向查找 （推荐）
</p>

&nbsp;

  * <span style="color: #dc5425;"><strong>Ctrl + R</strong>  反向查找命令</span>

<p style="padding-left: 30px;">
  快捷键Ctrl + R ，然后输入要查找的关键字，输入的同时，bash就会动态地增量搜索，找到想要的历史命令后可以按回车执行，或者esc把这条提取命令出来但是不执行。再按Ctrl + R 则继续往后查找符合条件的命令。
</p>

<p style="padding-left: 30px;">
  <img class="alignnone size-full wp-image-221" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/8DCD0B0A-2AE2-4353-B74F-948D69D15E29.jpg" alt="8DCD0B0A-2AE2-4353-B74F-948D69D15E29" width="474" height="33" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/8DCD0B0A-2AE2-4353-B74F-948D69D15E29-300x21.jpg 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/8DCD0B0A-2AE2-4353-B74F-948D69D15E29.jpg 474w" sizes="(max-width: 474px) 100vw, 474px" />
</p>

  * <span style="color: #dc5425;"><strong>history | grep keyword</strong>  列出所有符合条件的命令</span>

<p style="padding-left: 30px;">
  Ctrl + R 无疑是最方便常用的历史记录搜索方式，但是当然也可以用 history | grep keyword 来查找所有的符合条件的记录，然后再结合刚刚的! 方法完成命令。
</p>

&nbsp;

注意，以上所说的包含 ! 的技巧都是可以与别的命令拼接在一起的， 比如

<p style="padding-left: 30px;">
  sudo  !-3  , time  !472， sudo  !apt
</p>

&nbsp;

下面是一些关于历史记录的参数的技巧：

  * <span style="color: #dc5425;"><strong> !$</strong>  上一条命令的最后一条参数</span>

<p style="padding-left: 30px;">
  如果说你只想用上条命令的参数，一个个打出来又太繁复，就可以这样
</p>

<pre class="lang:default decode:true">cd !$</pre>

<p style="padding-left: 30px;">
  <img class="alignnone size-full wp-image-217" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/1D24DAF5-A00C-4255-A11D-C5DAE5BAD4BE.jpg" alt="1D24DAF5-A00C-4255-A11D-C5DAE5BAD4BE" width="618" height="143" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/1D24DAF5-A00C-4255-A11D-C5DAE5BAD4BE-300x69.jpg 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/1D24DAF5-A00C-4255-A11D-C5DAE5BAD4BE.jpg 618w" sizes="(max-width: 618px) 100vw, 618px" />
</p>

<p style="padding-left: 30px;">
  当然这种情形下也有更简单的方法，等讲到快捷键部分再说
</p>

&nbsp;

  * <span style="color: #dc5425;"><strong>!^</strong>  上一条命令的第一个参数</span>

<p style="padding-left: 30px;">
  $ 表示最后一个参数，而 ^就表示的是第一个参数
</p>

<p style="padding-left: 30px;">
  !^ 在这样的一个应用场景里十分方便：你刚备份了一个配置文件，然后想编辑这个配置文件
</p>

<pre class="lang:default decode:true">vim !^</pre>

<p style="padding-left: 30px;">
  <img class="alignnone size-full wp-image-219" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/C13F84B7-5386-4BF2-A2C0-7159434C053C.jpg" alt="C13F84B7-5386-4BF2-A2C0-7159434C053C" width="408" height="46" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/C13F84B7-5386-4BF2-A2C0-7159434C053C-300x34.jpg 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/C13F84B7-5386-4BF2-A2C0-7159434C053C.jpg 408w" sizes="(max-width: 408px) 100vw, 408px" />
</p>

  * <span style="color: #dc5425;"><strong>:n</strong>  第n个参数</span>

<p style="padding-left: 30px;">
  ^与$表示第一个参数和最后一个参数，而 :n 就表示第n个参数，比如 !:2就表示上一条命令的第2个参数
</p>

<pre class="lang:default decode:true">cd !:2</pre>

<p style="padding-left: 30px;">
  <img class="alignnone size-full wp-image-228" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/DA1A906D-34FE-4531-AB2F-891EE8AFB1F9.jpg" alt="DA1A906D-34FE-4531-AB2F-891EE8AFB1F9" width="361" height="58" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/DA1A906D-34FE-4531-AB2F-891EE8AFB1F9-300x48.jpg 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/DA1A906D-34FE-4531-AB2F-891EE8AFB1F9.jpg 361w" sizes="(max-width: 361px) 100vw, 361px" />
</p>

&nbsp;

注意，参数符号不仅是可以 !$、!^、!:n 这样用，这些关于参数的符号都是可以和!表达式任意组合使用的，比如

<p style="padding-left: 30px;">
  cd  !762:2 （表示762号历史命令的第2个参数）
</p>

<p style="padding-left: 30px;">
  ls  !-3^ （表示倒数第3个命令的第一个参数）
</p>

<p style="padding-left: 30px;">
  dpkg  -L  !apt$  (表示搜索含apt的命令的最后一个参数）
</p>

&nbsp;

  * <span style="color: #dc5425;"><strong>magic-space</strong>  让历史记录表达式和参数符号立即显出原形</span>

<p style="padding-left: 30px;">
  虽然历史记录表达式和参数符号使用起来简易方便，但是在包含这些表达式和符号的命令回车执行之前，你是并不知道这些表达式和符号到底代表的什么。为了解决这个问题，我们可以使用Magic-Space
</p>

<pre class="lang:default decode:true ">bind Space:magic-space</pre>

<p style="padding-left: 30px;">
  使用了这个设置后，在bash中输入历史记录表达式和参数符号后，按一下空格，这些表达式和符号就立即变成它们所代表的历史命令和参数，简称magic space。
</p>

<p style="padding-left: 30px;">
  可以把这句放到.bashrc中，让设置持久生效（Mac是在.bash_profile）。
</p>

&nbsp;

  * <span style="color: #dc5425;">命令前加空格，使之不计入history</span>

<p style="padding-left: 30px;">
  在命令前加空格，就可以避免改该命令计入history，小伙伴们就可以在不用清空history的前提下干一些坏事了。如果不行的话，可能需要设置 HISTCONTROL=ignorespace ，Ubuntu默认是可以用这个技巧的。
</p>

&nbsp;

  * <span style="color: #dc5425;">HISTSIZE=0  不记录命令</span>

<p style="padding-left: 30px;">
  如果不想记录命令，可以设置HISTSIZE=0。如果想恢复，可以在设置HISTSIZE为一个大于零的值（默认为500或者1000）
</p>

&nbsp;

  * <span style="color: #dc5425;">HISTCONTROL=ignoredups  去除重复命令</span>

<p style="padding-left: 30px;">
  这样设置后，多次的同样的命令连续执行就会只记录一次。
</p>

&nbsp;

历史记录篇就暂时介绍到这，下篇快捷键篇会介绍更多Bash的技巧。

如果你知道更多关于Bash 命令历史的操作技巧，请在评论区分享给大家。