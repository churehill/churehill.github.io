---
id: 157
title: 修改了xiaoxia的sogou代理脚本
date: 2014-03-30T13:32:33+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=157
permalink: /tech/sogou-script/
duoshuo_thread_id:
  - 1340357450817077257
categories:
  - Tech
tags:
  - ipv6
  - network
  - python
---
修改了xiaoxia的sogou代理脚本使之能在windows下bind 127.0.0.1

orz...首先要说的是 乱google了一天，和乱下载各种python反编译程序，以及各种配置环境后，才发现问题的关键。。。。。。

本弱本来用的是一个学长（已毕业）用pyinstaller编译sogou代理脚本出的可执行程序，后来因为某些原因需要更改里面的一些东西，我就想要使用没有编译的sogou python代理脚本，直接用python运行，改了一些想要的东西后，成功运行。。。可是不久就发现问题了。。。

下面的分割线之间的都是些奇怪的东西，不喜的可以直接跳过分割线中间的东西，直接看结论。。。

* * *

&nbsp;

首先，感谢 xiaoxia大神的sogou代理脚本，<a href="http://xiaoxia.org/2011/11/14/update-sogou-proxy-program-with-https-support/" target="_blank">原地址</a>

然后，讲一讲我这一天坑爹的乱改。。。。

这个python脚本bind了地址""，也就是默认地址，就是localhost，但是如果浏览器代理设置成127.0.0.1，就不管用，按理说localhost应该与127.0.0.1一样呀，可是在windows下面就是给跪了，不一样orz，在mac和linux下试了是可以的，就是脚本中代理地址bind的是"", 然后就可以用127.0.0.1连上代理，自行google了一番也没找到解释（果然搜索能力太弱了吗...呜呜呜，答案后面揭晓），我当时是以为是python的问题，还向一个同学吐槽windows上的python实现太差（orz, 后来发现当然不是python的缘故，我真的是拙计，蠢到会认为python会在这个问题上出错）。

刚开始，我的思路就是google一下127.0.0.1和localhost在python里面的区别，然后解决一下。后来直接想bind 127.0.0.1，结果出现这样的错误

[<img class="alignnone size-medium wp-image-158" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/20140330110110453-300x105.png" alt="20140330110110453" width="300" height="105" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/20140330110110453-300x105.png 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/20140330110110453.png 629w" sizes="(max-width: 300px) 100vw, 300px" />](http://blog.tpircsboy.com/wp-content/uploads/2015/02/20140330110110453.png)

出现socket错误 10049, 大概意思就是目标地址不可以被分配，关于windows下的socket错误代码可以参加<a href="http://msdn.microsoft.com/en-us/library/windows/desktop/ms740668(v=vs.85).aspx" target="_blank">这里</a>。

看到这里顿时就崩溃了，，为啥不行，orz，再次google查python不能bind 127.0.0.1，无果 T_T

然后，换了一种思路，想看一下学长编译好的python可执行程序为什么可以既bind上localhost，又bind上127.0.0.1。于是就是各种查python编译与反编译，因为原来对python的编译并无了解，就不知道学长用的那种python编译方法，是py2exe，还是pyinstalller。

于是截图那个可执行程序的图标[<img class="alignnone size-full wp-image-159" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/20140330111356421.png" alt="20140330111356421" width="43" height="46" />](http://blog.tpircsboy.com/wp-content/uploads/2015/02/20140330111356421.png)，然后百度图片搜索，无果（百度果然比不上google），google搜图，发现是pyinstaller生成的默认图标。

于是，我就开始找pyinstaller反编译的程序，在stackoverflow上找到了许多链接，找到了几个比较好的：

<a href="http://sourceforge.net/projects/easypythondecompiler/?source=dlp" target="_blank">Easy Python Decomplier</a> 能够把.pyc文件转回.py，界面挺不错；

<a href="http://sourceforge.net/projects/pyinstallerexerebuilder/?source=dlp" target="_blank">Pyinstaller Exe Rebuilder</a>，Qt写的界面，然后内置了Easy Python  Decomplier的功能，可以直接打开python编译好的exe，然后显示出里面的python代码，而且可以修改后自动重新编译成exe，但是有很大的缺陷，不知是pyinstaller更改了编译方式，还是怎么样，反正这个软件只显示了exe中out00-PYZ.pyz\_extracted文件夹中pyc反编译的结果，而事实上现在的pyinstaller版本生成的exe中在out00-PYZ.pyz\_extracted文件夹外才是关键的代码，文件夹中的代码只是python的一些库，比如encoding、socket等等，于是pyinstaller exe rebuilder只显示了这些库的代码，（orz，坑了我好长时间，一直在找这些库的代码跟我用的库的代码的不同T_T）；

<a href="http://sourceforge.net/projects/pyinstallerextractor/?source=dlp" target="_blank">PyInstaller Extractor</a>：从pyinstaller exe rebuilder的帮助中看到的，是一个python写的脚本，单文件，可以把python编译好的exe按exe中的目录生成出来各种文本和pyc，我用这个脚本解开了学长编译好的exe，然后才发现pyinstaller exe rebuilder的坑，就是核心代码在那个out00-PYZ.pyz\_extracted的外面用纯文本保存着。。。。，发现这个恍然大悟，打开核心代码只是粗略的看一下，直接看了bind那一点，也就是serveraddress = ("",1998), 跟我的一样（结果没有仔细看那个核心代码，不然就可以直接得出结论了。。。下次干这类调查的事情一定要稳住仔细看，不然又要浪费好多时间T\_T），结果就是我没发现要点。。。。

&nbsp;

再往后，因为我没发现核心代码的要点，我就怀疑是不是python代码在编译后是不是用了不同的库实现，让bind windows的localhost时也bind 127.0.0.1。（果然我就是一个弱，后来才知道python在windows上socket是调用系统API，不管是编译前还是编译后，所以说编译前后都一样），抱着刚刚的那个想法，下载了pywin32、pyinstaller，去编译我的修改后的python脚本，编译好后，运行，还是只能bind localhost，不能bind 127.0.0.1 。。。（顿时感觉一片灰暗。。）

后来，我决定按着上面的socket error10049出错的信息，一行行排查，我竟然想到 手动修改python的库。。（真是初生牛犊不怕虎orz), 在SocketServer.py, socket.py出错的地方加入print 语句，看一下中间会不会哪地方传递参数出错，结果追查到return getattr(self._sock,name)(*args) ，发现完全没错误，再往下盘查就是python的c代码实现了，是调用系统的api。orz，对自己太失望了，完全没辙了。 不过也算是看了一部分python的源码，嘿嘿，涨姿势。

&nbsp;

* * *

&nbsp;

最后几近放弃，决定重头开始，我就看别人是怎么写python 的socket，看了几个，没看出门道。。。。。看到<a href="http://www.oschina.net/question/12_76126" target="_blank">一个写的不错的</a>，终于决定手敲一下试一试，敲到这一句，s = socket.socket(socket.AF\_INET, socket.SOCK\_STREAM)，诶，这个socket.AF\_INET是啥，（果然代码不能只看，要自己手动边敲边学），好像在xiaoxia的python脚本里见到过，，然后我按照刚刚那个教程继续写，结果就成功bind 了127.0.0.1，orz，什么状况，赶快去对比xiaoxia大神写的脚本，发现他是这样写的。。。address\_family = socket.AF_INET6 ，，，多了一个数字6，难道是ipv6，赶快去查了查，关于这个的资料不多，不过真的是ipv6，

这时我才真正的是恍然大悟，ipv6, T_T,怪不得能用localhost而不能用127.0.0.1，因为127.0.0.1在ipv6是不合法的。。。在ipv6中localhost是[::] ,这下全明白了，怪不得一直出现socket error 10049，因为127.0.0.1在ipv6上是不合法的地址，怪不得许多软件设置了localhost进行代理也不能用，因为这些软件是不支持ipv6的。。。果断进行以下的更改：

<pre class="lang:python decode:true">class ThreadingHTTPServer(ThreadingMixIn, HTTPServer): 
    address_family = socket.AF_INET6</pre>

改为：

<pre class="lang:python decode:true ">class ThreadingHTTPServer(ThreadingMixIn, HTTPServer): 
    address_family = socket.AF_INET</pre>

&nbsp;

orz，这样就行了，用ipv4，因为sogou代理服务器是ipv4的，我估计xiaoxia大神当时写这个的时候没想太多，因为是教育网，直接就用了AF_INET6。这样改了之后，就可以bind 127.0.0.1了，赞！。
  
回头去看学长的代码。。发现学长也发现了这一点，他也改了xiaoxia写的脚本的这一点，这也是他的编译好的exe能够bind 127.0.0.1的原因，orz, 都怪我刚刚没仔细看学长的核心代码，这就是我刚刚说的要点，否则也不用费这么大功夫了T_T。。。

后来，发现改成AF_INET后，直接写bind ""就行了( serveraddress = ("",1998) ),这样既可以用localhost，也可以用127.0.0.1连接上代理。

总之，就这么DT。。。。。。T\_T T\_T T\_T T\_T T\_T T\_T T_T ,哎，不过也也算涨了很多见识。。涨姿势！！

另外也可以快乐的使用sogou代理了，，，啦啦

&nbsp;