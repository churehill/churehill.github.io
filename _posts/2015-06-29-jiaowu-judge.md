---
id: 282
title: 西工大教务系统自动评教脚本javascript版
date: 2015-06-29T02:06:38+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=282
permalink: /tech/jiaowu-judge/
duoshuo_thread_id:
  - 1340357450817077278
categories:
  - Tech
---
### 直接上代码：

<pre class="toolbar:1 wrap:true lang:js decode:true ">var arr =  document.getElementsByName('bottomFrame')[0].contentDocument.getElementsByName('mainFrame')[0].contentDocument.getElementsByTagName('img');for (var i = 0; i &lt; arr.length; i++) { var nd=arr[i]; if(nd.src.match("edit.gif$") != "edit.gif") continue; var tmp=nd.name.split('#@'); var http=new XMLHttpRequest; http.open('POST', "/jxpgXsAction.do?oper=wjpg", true); http.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); http.send("wjbm=" + tmp[0] + "&bpr=" + tmp[1] + "&pgnr=" + tmp[5] + "&0000000093=10_1&0000000094=10_1&0000000095=10_1&0000000096=10_1&0000000097=10_1&0000000098=10_1&0000000099=10_1&0000000100=10_1&0000000101=10_1&0000000102=10_1&zgpj=%C2%FA%D2%E2");}
</pre>

### **用法：**

打开教务系统，进入到这个页面

[<img class="alignnone size-full wp-image-283" src="http://blog.tpircsboy.com/wp-content/uploads/2015/06/jiaowu.jpg" alt="jiaowu" width="912" height="340" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/06/jiaowu-300x112.jpg 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/06/jiaowu.jpg 912w" sizes="(max-width: 912px) 100vw, 912px" />](http://blog.tpircsboy.com/wp-content/uploads/2015/06/jiaowu.jpg)

然后，在浏览器栏输入 **_javascript:_** ，然后粘贴入上面的代码，回车就行了

&nbsp;

### 说明：

每题都是最好的选项，评语是“满意”。

之所以让输入javascript:这几个字符而不是把这几个字符放到代码中，是因为现代浏览器往浏览器栏粘入东西时，浏览器会自动过滤javascript这几个字符。

### 兼容性：

在Chrome和Firefox上都测试过了，IE没测试

### 源起：

前一段看一些学弟在搞教务系统的评教脚本，想了想我以前也想写个自动评教脚本，可是后来一直没写（哎，略懒散）。学弟们用的是Python，可是我感觉这种情况下还是用javascript最方便，毕竟可以直接在浏览器中执行，然后查看结果。所以，趁杀气腾腾的考试周刚过去就写了这个脚本。

### 格式化后的代码：

上面贴出的是一行的代码，方便粘贴，下边的是格式化后的代码

<pre class="lang:js decode:true ">var arr = document.getElementsByName('bottomFrame')[0].contentDocument.getElementsByName('mainFrame')[0].contentDocument.getElementsByTagName('img');
for (var i = 0; i &lt; arr.length; i++) {
    var nd = arr[i];
    if (nd.src.match("edit.gif$") != "edit.gif") continue;
    var tmp = nd.name.split('#@');
    var http = new XMLHttpRequest;
    http.open('POST', "/jxpgXsAction.do?oper=wjpg", true);
    http.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    http.send("wjbm=" + tmp[0] + "&bpr=" + tmp[1] + "&pgnr=" + tmp[5] + "&0000000093=10_1&0000000094=10_1&0000000095=10_1&0000000096=10_1&0000000097=10_1&0000000098=10_1&0000000099=10_1&0000000100=10_1&0000000101=10_1&0000000102=10_1&zgpj=%C2%FA%D2%E2");
}</pre>

&nbsp;

### 吐槽：

这老教务系统网页iframe套iframe也是任性，听说新教务系统正在写，所以不知道这份代码能用多久。 ╮(╯▽╰)╭