---
id: 252
title: TopCoder插件Greed配置及自定义代码模板
date: 2014-12-24T11:07:21+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=252
permalink: /tech/topcoder-greed/
duoshuo_thread_id:
  - 1340357450817077276
categories:
  - Tech
---
TopCoder Arena是Java写的，操作、看题都不方便，而且自带编辑器很难用。但是TopCoder Arena可以自定义插件，这是非常好的地方。参加TopCoder的coder一般都会用一些插件，从Petr公开的他自己打TopCoder的视频看，他是自己写的插件。当然，我们没必要也自己写插件，因为有一些超好用的插件供我们使用。

我以前用的是传统插件组合方案：CodeProcessor+FileEdit+一款数据生成器

但是这些插件组合还比不上一个插件awesome，这个插件就是Greed 2.0，它是开源的，托管在[github上](https://github.com/shivawu/topcoder-greed)。这个插件涵盖了传统插件组合方案的所有功能，集成一体，配置简单，并且提供超级强大的自定义功能。

安装的话，可以参照Greed项目首页的[教程](https://github.com/shivawu/topcoder-greed#quick-start)：

<ol class="task-list">
  <li>
    Download <a href="https://github.com/shivawu/topcoder-greed/releases/download/2.0-RC/Greed-2.0-RC-7.1.0.jar">the single Greed jar</a>. The binary is compatible with Java 1.6+.  要把jar文件放在一个固定的位置，TopCoder Arena是按文件路径记录配置的。
  </li>
  <li>
    Open <strong>Topcoder arena</strong> -> <strong>Login</strong> -> <strong>Options</strong> -> <strong>Editor</strong> -> <strong>Add</strong><br /> <a href="http://blog.tpircsboy.com/wp-content/uploads/2015/02/TC-Add-Plugin.png"><img class="alignnone size-full wp-image-254" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/TC-Add-Plugin.png" alt="TC-Add-Plugin" width="474" height="167" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/TC-Add-Plugin-300x106.png 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/TC-Add-Plugin.png 474w" sizes="(max-width: 474px) 100vw, 474px" /></a><br /> Done! Remember to check <strong>Default</strong> and <strong>At startup</strong>.
  </li>
  <li>
    Click <strong>Configure</strong>.<br /> <a href="http://blog.tpircsboy.com/wp-content/uploads/2015/02/Set-Workspace.png"><img class="alignnone size-full wp-image-255" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/Set-Workspace.png" alt="Set-Workspace" width="626" height="56" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/Set-Workspace-300x27.png 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/Set-Workspace.png 626w" sizes="(max-width: 626px) 100vw, 626px" /></a><br /> Fill in your workspace full path, make sure it's a existing directory.
  </li>
  <li>
    All set! Go get your rating! <a href="http://blog.tpircsboy.com/wp-content/uploads/2015/02/UI-Look.png"><img class="alignnone size-full wp-image-256" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/UI-Look.png" alt="UI-Look" width="1396" height="299" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/UI-Look-300x64.png 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/UI-Look-1024x219.png 1024w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/UI-Look.png 1396w" sizes="(max-width: 1396px) 100vw, 1396px" /></a>
  </li>
</ol>

这样，Greed 2.0就安装好了，现在打开SRM的一道题，Greed就会在workspace里把题面保存成HTML，按模板生成好代码和测试数据。

* * *

&nbsp;

安装后，我们需要自定义代码模板，比如说加几个头文件，添一些宏定义，加一些调试信息。

Greed的自定义功能很厉害的，甚至可以做到自定义生成题目代码文件后执行脚本命令，它用了一个强大的配置文件解析库[typesafe-config](https://github.com/typesafehub/config)，这个库使用的是类似json的配置结构和语法，默认配置和自定义配置的叠加很方便，默认配置类似创建json对象，自定义配置就相当于修改一个json对象。当然，这些都是我看了一些Greed的代码和typesafe-config的项目介绍后知道的，仅仅是看完Greed的[Template Definition](https://github.com/shivawu/topcoder-greed#template-definition)，不太容易搞懂，我把里面的那段代码放进自定义配置，结果Greed显示配置错误，这是因为这段代码是创建json对象，而自定义配置应该是修改json对象。

仔细看了一些Greed的代码和了解了一下typesafe-config后，我知道了正确的自定义代码模板的方法，参照Greed github上的[默认配置](https://github.com/shivawu/topcoder-greed/blob/master/src/main/resources/default.conf)，正确的配置方法如下：

1.  在上面设置的workspace目录下面创建 greed.conf 文件，里面写如下内容

<pre class="lang:default decode:true">greed.language.cpp.templateDef.source.templateFile = cpp.tmpl
greed.language.java.templateDef.source.templateFile = java.tmpl
</pre>

这两行就如同json对象的赋值一样，指定c++的代码模板文件为cpp.tmpl，指定java的代码模板文件为java.tmpl

2.  然后创建 cpp.tmpl, java.tmpl 文件，也是在workspace目录下，这两个文件的内容就放我们自定义的模板。但是要注意必须要用到一些Greed预定义的模板代码，比如${CutBegin}, ${CutEnd}, ${<TestCode}。
  
所以，我们可以参照着[默认的cpp模板文件](https://github.com/shivawu/topcoder-greed/blob/master/src/main/resources/templates/source/cpp.tmpl)来写我们自己的cpp模板文件cpp.tmpl

<pre class="lang:default decode:true">#include &lt;cstdio&gt;
#include &lt;cmath&gt;
#include &lt;cstring&gt;
#include &lt;ctime&gt;
#include &lt;iostream&gt;
#include &lt;algorithm&gt;
#include &lt;bitset&gt;
#include &lt;string&gt;
#include &lt;set&gt;
#include &lt;stack&gt;
#include &lt;map&gt;
#include &lt;queue&gt;
#include &lt;vector&gt;
#include &lt;unordered_set&gt;
#include &lt;sstream&gt;
#include &lt;typeinfo&gt;
#include &lt;fstream&gt;

using namespace std;

class ${ClassName} {
    public:
    ${Method.ReturnType} ${Method.Name}(${Method.Params}) {
        return ${Method.ReturnType;zeroval};
    }
};

${CutBegin}
${&lt;TestCode}
${CutEnd}
</pre>

参照[java的默认模板文件](https://github.com/shivawu/topcoder-greed/blob/master/src/main/resources/templates/source/java.tmpl)来写我们自己的模板文件java.tmpl

<pre class="lang:default decode:true">import java.io.*;
import java.util.*;
import java.math.*;

public class ${ClassName} {
	public ${Method.ReturnType} ${Method.Name}(${Method.Params}) {
		return ${Method.ReturnType;zeroval};
	}

${CutBegin}
${&lt;TestCode}
${CutEnd}
}
</pre>

&nbsp;

3.  在TC Arena的查看题目的窗口里找到Greed的Console，然后点击Reload Configuration和Regenerate code，或者直接重启TC客户端也可以。
  
[<img class="alignnone size-full wp-image-256" src="http://blog.tpircsboy.com/wp-content/uploads/2015/02/UI-Look.png" alt="UI-Look" width="1396" height="299" srcset="http://blog.tpircsboy.com/wp-content/uploads/2015/02/UI-Look-300x64.png 300w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/UI-Look-1024x219.png 1024w, http://blog.tpircsboy.com/wp-content/uploads/2015/02/UI-Look.png 1396w" sizes="(max-width: 1396px) 100vw, 1396px" />](http://blog.tpircsboy.com/wp-content/uploads/2015/02/UI-Look.png)

这样，如果Greed没显示什么错误信息就算自定义代码模板配置完成了。

&nbsp;

当然，我其实是一个TC弱渣，T_T