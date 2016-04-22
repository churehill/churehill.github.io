---
id: 27
title: Codeforces Round 264
date: 2014-08-31T00:31:31+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=27
permalink: /acm/codeforces-round-264/
duoshuo_thread_id:
  - 1340357450817077252
categories:
  - ACM
tags:
  - ACM
  - CodeForces
---
&nbsp;

* * *

### [C - Gargari and Bishops](http://codeforces.com/contest/463/problem/C)【预处理，黑白棋】

这题比赛的时候漏看这一句：

> Gargari wants to place two bishops on the chessboard in such a way that _there is no cell that is attacked by both of them_. 

就是说任意一个位置不能被两个主教同时攻击。结果当时比赛时思路大致想对了，但是一直在想怎么减掉两个主教发射出的斜线相交处的数值，orz。
  
以下是我当时的代码：

<pre class="lang:c++ decode:true ">#include &lt;iostream&gt;
#include &lt;cstdio&gt;
#include &lt;cstdlib&gt;
#include &lt;cstring&gt;
#include &lt;cmath&gt;
#include &lt;string&gt;
#include &lt;vector&gt;
#include &lt;queue&gt;
#include &lt;stack&gt;
#include &lt;algorithm&gt;
#include &lt;climits&gt;
using namespace std;
typedef long long LL;

int ar[2500][2500];
LL le[4500],re[4500];

int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin&gt;&gt;n;
    for(int i=1;i&lt;=n;i++) {
        for(int j=1;j&lt;=n;j++) {
            cin&gt;&gt;ar[i][j];
            le[i+j]+=ar[i][j];
            re[i-j+n]+=ar[i][j];
        }
    }
    LL fm=-1,sm=-1;
    int fx=1,fy=1,sx=2,sy=1;
    LL temp = -1;
    for(int i=1;i&lt;=n;i++) {
        for(int j=1;j&lt;=n;j++) {
            temp = le[i+j]+re[i-j+n] - ar[i][j];
            if(temp &gt; fm) {
                fm = temp;
                fx=i,fy=j;
            }
        }
    }
    LL ans = fm;
    temp = -1;
    le[fx+fy]=-1;
    re[fx-fy+n]=-1;
    for(int i=1;i&lt;=n;i++) {
        for(int j=1;j&lt;=n;j++) {
            temp = le[i+j]+re[i-j+n] - ar[i][j];
            if(temp &gt; sm) {
                sm = temp;
                sx=i,sy=j;
            }
        }
    }
    ans += sm;
    cout&lt;&lt;ans&lt;&lt;"\n";
    cout&lt;&lt;fx&lt;&lt;" "&lt;&lt;fy&lt;&lt;" "&lt;&lt;sx&lt;&lt;" "&lt;&lt;sy; 
}</pre>

后来，看了[hcbbt](http://blog.csdn.net/hcbbt/article/details/38947563)的题解时，恍然大悟：

> 题意：
    
> 给你一个棋盘，格子上有些value，然后你要选择两个位置放棋子：
> 
>   1. 一个棋子能沿着对角线走，并吃掉格子上的value
>   2. 任意一个格子不能同时被两个棋子走到，就是说value不能重复吃
> 
> 问能吃到的最大value和，以及两个棋子的位置。
> 
> 分析：
    
> 对于规则2，就像黑白棋一样，只要放的颜色不一样就可以了，也就是(x+y)%2不一样就行了。
> 
> 接下来就是求value和了。
    
> 棋盘大小为2000*2000，如果暴力每个格子放棋子能吃到的value和会超时。
    
> 很明显，(x,y)的value和就等于它所属的对角线和+斜对角线和-value(i,j)就行了。
    
> 我们只要预处理出每条对角线和斜对角线的和就行了。
    
> 我们发现(x+y)相同的格子都属于同个对角线，(x-y)相同的属于同个斜对角线。我们开两个数组来记录就行了，由于x-y会有负数，我们给它们+2000就行了。
> 
> 这样，计算某个格子的value和的时候，直接取(x+y)对角线和及(x-y)斜对角线来做就行了。 

这个**黑白棋**的思路真的很赞。
  
按照这个黑白棋的思路区分两个主教，改了代码,然后就对了：

<pre class="lang:c++ decode:true ">#include &lt;iostream&gt;
#include &lt;cstdio&gt;
#include &lt;cstdlib&gt;
#include &lt;cstring&gt;
#include &lt;cmath&gt;
#include &lt;string&gt;
#include &lt;vector&gt;
#include &lt;queue&gt;
#include &lt;stack&gt;
#include &lt;algorithm&gt;
#include &lt;climits&gt;
using namespace std;
typedef long long LL;

int ar[2500][2500];
LL le[4500],re[4500];

int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin&gt;&gt;n;
    for(int i=1;i&lt;=n;i++) {
        for(int j=1;j&lt;=n;j++) {
            cin&gt;&gt;ar[i][j];
            le[i+j]+=ar[i][j];
            re[i-j+n]+=ar[i][j];
        }
    }
    LL fm=-1,sm=-1;
    int fx=2,fy=1,sx=1,sy=1;
    LL temp = -1;
    for(int i=1;i&lt;=n;i++) {
        for(int j=1;j&lt;=n;j++) {
            temp = le[i+j]+re[i-j+n] - ar[i][j];
            if((i+j)%2) {
                if(temp &gt; fm) {
                    fm = temp;
                    fx = i,fy = j;
                }
            }
            else {
                if(temp &gt; sm) {
                    sm = temp;
                    sx = i,sy = j;
                }
            }
        }
    }
    cout&lt;&lt;fm+sm&lt;&lt;"\n";
    cout&lt;&lt;fx&lt;&lt;" "&lt;&lt;fy&lt;&lt;" "&lt;&lt;sx&lt;&lt;" "&lt;&lt;sy; 
}</pre>

&nbsp;

* * *

### [D - Gargari and Permutations](http://codeforces.com/contest/463/problem/D)【多序列LCS，DAG】

这道题我比赛的时候想错了，我当时的想法是：求前两个序列的LCS，然后依次跟剩下的序列用生成的LCS往下求，结果WA了。

以下题解来自[hcbbt](http://blog.csdn.net/hcbbt/article/details/38947563)的blog

> 题意:
    
> 求k个长度为n的序列的最长公共子序列。(2<=k<=5)
> 
> 分析：
    
> 不能求前两个序列的LCS，然后拿结果去跟下面的求。
    
> 因为前两个序列的LCS是不唯一的。
> 
> 我们预处理(i,j)，如果对于每个序列都有pos[i] < pos[j]，就说明作为LCS的话，i后面可以跟j，然后在i,j间连一条边。
    
> 这样就会转化为一个DAG了，求下最长路就行了。 

不过我按照[hcbbt](http://blog.csdn.net/hcbbt/article/details/38947563)的代码自己写了一份：

<pre class="lang:c++ decode:true ">#include &lt;iostream&gt;
#include &lt;cstdio&gt;
#include &lt;cstdlib&gt;
#include &lt;cstring&gt;
#include &lt;string&gt;
#include &lt;vector&gt;
#include &lt;queue&gt;
#include &lt;stack&gt;
#include &lt;algorithm&gt;
#include &lt;climits&gt;
using namespace std;
typedef long long LL;

int n,k;
int ar[5][1100];
vector&lt;vector&lt;int&gt; &gt; vec(1100);
int dis[1100];

bool check(int x,int y)
{
    for(int i=0;i&lt;k;i++) {
        if(ar[i][x]&gt;=ar[i][y])
            return 0;
    }
    return 1;
}

int dfs(int x)
{
    if(dis[x]) return dis[x];
    int temp = 0;
    for(int i=vec[x].size()-1;i&gt;=0;i--) {
        temp = max(temp,dfs(vec[x][i]));
    }
    return dis[x]=temp+1;
}

int main()
{
    ios::sync_with_stdio(0);
    cin&gt;&gt;n&gt;&gt;k;
    int temp;
    for(int i=0;i&lt;k;i++) {
        for(int j=1;j&lt;=n;j++) {
            cin&gt;&gt;temp;
            ar[i][temp] = j;
        }
    }

    for(int i=1;i&lt;=n;i++) {
        for(int j=1;j&lt;=n;j++) {
            if(i!=j&&check(i,j))
                vec[i].push_back(j);
        }
    }
    int ans = 0;
    for(int i=1;i&lt;=n;i++) {
        if(!dis[i])
            ans = max(dfs(i),ans);
    }
    cout&lt;&lt;ans;

}</pre>

&nbsp;