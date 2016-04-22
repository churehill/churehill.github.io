---
id: 178
title: UVa 112 Tree Summing
date: 2013-08-07T13:15:26+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=178
permalink: /acm/uva-112-tree-summing/
duoshuo_thread_id:
  - 1340357450817077271
categories:
  - ACM
tags:
  - cin
  - 二叉树
  - 输入技巧
---
这一题题意和思路都不是很难，不用建立一棵二叉树，直接进行遍历并记录节点之前包括该节点的元素之和。不过因为空格的原因，输入处理很麻烦，以前竟不知道cin.peek()和cin.ignore()，还有cin.putback()函数。看了<a href="https://github.com/igorbonadio/uva/tree/master/112" target="_blank">https://github.com/igorbonadio/uva/tree/master/112</a>上的代码才知道这回事，比直接取字符判断方便多了。这一题关键要注意空格，负号的处理，还有0()的情况。

以下是代码：

<pre class="lang:c++ decode:true">#include &lt;iostream&gt;
#include &lt;string&gt;
#include &lt;fstream&gt;

using namespace std;
int var;
bool ans;
void rmspace()
{
    while(cin.peek()==' '||cin.peek()=='\n')
        cin.ignore(1);
}
bool node(int sum)
{
    int v,sgn =1;
    rmspace();
    cin.ignore(1);// '('
    rmspace();
    if(cin.peek()==')')
    {
        cin.ignore(1); //")"
        return sum==var;
    }
    if(cin.peek()=='-')
    {
        cin.ignore(1);
        sgn = -1;
    }
    rmspace();
    cin&gt;&gt;v;
    v *= sgn;
    sum+=v;
    bool l = node(sum);
    bool r = node(sum);
    if(l&&r)
        ans = true;
    rmspace();
    cin.ignore(1);// ')'
    return false;//仅上对叶子进行判断是否等于var，非叶子用false直接跳过
}

int main()
{
    //freopen("in.txt","r",stdin);
    //freopen("out.txt","w",stdout);
    while(cin&gt;&gt;var)
    {
        ans = 0;
        node(0);
        cout&lt;&lt;(ans?"yes":"no")&lt;&lt;endl;
    }
}</pre>

还有测试数据：

<pre class="lang:default decode:true ">Sample Input
22 (5(4(11(7()())(2()()))()) (8(13()())(4()(1()()))))
20 (5(4(11(7()())(2()()))()) (8(13()())(4()(1()()))))
10 (3
(2 (4 () () )
(8 () () ) )
(1 (6 () () )
(4 () () ) ) )
5 ()
0 ()
5 (5 () ())
5 ( 5 () () )
5 (1 (3 () ()) (4 () ()))
5 (18 ( - 13 ( ) ( ))())
0 (1 ()(-2 () (1()()) ) )
2 (1 () (1 () (1 () () ) ) )
10 (5 () (5 () (5 () (5 () (4 () () ) ) ) ) )
10 (5 () (5 () (5 () (5 ( 3 () () ) (4 () () ) ) ) ) )
20 (5 () (5 () (5 () (5 () (4 () () ) ) ) ) )
Sample Output
yes
no
yes
no
no
yes
yes
yes
yes
yes
no
no
no
no
注：测试数据来自crystal_yi</pre>

&nbsp;

<div class="dp-highlighter bg_cpp">
</div>