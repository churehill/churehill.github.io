---
id: 344
title: 使用Pycharm进行Python远程开发
date: 2016-04-05T18:39:35+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=344
permalink: /tech/pycharm_remote_development/
duoshuo_thread_id:
  - 6270013036736545537
categories:
  - Tech
---
最近用Python开发Linux系统应用，可是主力机不是Linux，没法直接写码一键运行测试，只能把代码跑在VMware的Linux虚拟机中，把工程目录设置为共享目录，这样主力系统和虚拟机都能访问。所以，一边在主力系统中用Pycharm写代码，一边开着iTerm终端用ssh登进虚拟机，更改代码后用终端部署代码到测试目录中运行，查看输出，然后再改代码，就这样循环下去。虽然有双屏幕好一点，但是这样搞了两天，我就感觉一直重复手敲命令部署代码然后运行太烦了。而Pycharm的远程开发能够很好解决这样的应用场景。

Pycharm不仅可以使用本地python解释器，还可以通过ssh使用远程python解释器，而且可以帮你自动部署代码到服务器上，这样就能实现在Pycharm项目使用虚拟机中的环境依赖，写好代码然后直接run，Pycharm自动远程在虚拟机中部署运行代码。

配置Pycharm远程开发的大致步骤就是

<p style="padding-left: 30px;">
  配置Deployment：在 <em>Tools | Deployment | Configuration </em>配置Connection 和 Mappings
</p>

<p style="padding-left: 30px;">
  配置自动上传项目文件：勾上 <em>Tools | Deployment | Automatic Upload</em>
</p>

<p style="padding-left: 30px;">
  设置远程Python解释器：在 <em>File | Settings (Preferences for Mac OS) | Project | Project Interpreter </em>添加 Remote Interpreter，选择使用Deployment配置，再填写Python Interpreter路径
</p>

<p style="padding-left: 30px;">
  配置Run Configuration：在 <em>Run | Edit Configurations </em>添加 Python 配置，然后选择Remote Interpreter（项目默认）
</p>

具体可以参见：

<blockquote data-secret="giAAo9q3ym" class="wp-embedded-content">
  <p>
    <a href="http://blog.jetbrains.com/pycharm/2015/03/feature-spotlight-python-remote-development-with-pycharm/">Feature Spotlight: Python remote development with PyCharm</a>
  </p>
</blockquote>



配置完成后，在Pycharm中点Run，项目就会在虚拟机中部署运行，输出运行结果，也可以进行Debug，就如同是跑在本地一样。

&nbsp;

另外，如果项目需要root权限才能正常运行，除了让Pycharm用root连接服务器，还可以这样：

配置Deployment就用常规账户，然后将这个账户加入sudo组，再配置sudo免密码，

创建一个sudo运行python的shell脚本，保存在服务器上（注意这个脚本的文件名必须是以python开头，不然pycharm不将其识别为Python解释器）

<pre class="lang:sh decode:true " title="Python with sudo">#!/bin/bash
sudo -E env PYTHONPATH=$PYTHONPATH python3 "$@"</pre>

最后，在Pycharm中配置远程Python解释器，_Python Interpreter Path_就填这个shell脚本的路径，这样Pycharm就会将其当成一个Python解释器，然后就能root运行项目了。

解释一下这个脚本，由于sudo不会传递环境变量，而pycharm在运行项目前会设置一些变量，比如PYTHONUNBUFFERED、PYCHARM\_HOSTED、PYTHONIOENCODING、JETBRAINS\_REMOTE_RUN、PYTHONPATH，没有这些变量项目运行就会有问题，比如找不到sources目录。sudo的-E参数能将一部分参数传递过去，但不会将PATH相关的变量传递过去，对于PYTHONPATH就需要env手动设置传递过去。这样，用root权限Python运行的时候就能接触到这些环境变量，项目才能够正常运行。

其实，主要是PYTHONPATH在起作用，因为PYTHONPATH影响Python的模块加载路径。

参考：<http://stackoverflow.com/questions/14299509/debugging-in-pycharm-with-sudo-privileges>

&nbsp;