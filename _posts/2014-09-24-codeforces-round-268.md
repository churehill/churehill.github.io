---
id: 39
title: Codeforces Round 268
date: 2014-09-24T20:05:13+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=39
permalink: /acm/codeforces-round-268/
duoshuo_thread_id:
  - 1340357450817077249
categories:
  - ACM
tags:
  - ACM
  - CodeForces
---
* * *

## [D. Two Sets](http://codeforces.com/contest/468/problem/B)

题意：给出n个数和a和b，要求x在集合A的话，a-x也要在集合A；x在集合B的话，b-x也要在集合B。求出可行的集合分配，或输出无解。

由于这几天一直在看强连通分量和2-SAT，所以看到这一题第一印象就是这是道2-SAT模板题，所以直接上了模板，简单分析了一下如何加边，然后交上去就过了pretest，但是最后Fail System Test了。

如下是我的代码：

<pre class="lang:c++ decode:true ">#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;cstring&gt;
#include &lt;map&gt;
#include &lt;set&gt;
#include &lt;cstdio&gt;
using namespace std;

const int MAX_V = 200500;
int V;
vector&lt;int&gt; G[MAX_V],rG[MAX_V];
vector&lt;int&gt; vs;
bool used[MAX_V];
int cmp[MAX_V];

void add_edge(int f,int t)
{
    G[f].push_back(t);
    rG[t].push_back(f);
}

void dfs(int v)
{
    used[v] = true;
    for(int i=G[v].size()-1;i&gt;=0;i--) {
        if(!used[G[v][i]]) dfs(G[v][i]);
    }
    vs.push_back(v);
}

void rdfs(int v,int k)
{
    used[v] = true;
    cmp[v] = k;
    for(int i=rG[v].size()-1;i&gt;=0;i--) {
        if(!used[rG[v][i]]) rdfs(rG[v][i],k);
    }
}

// set V , clear G and rG, add edges, then scc 
int scc()
{
    memset(used,0,sizeof used);
    vs.clear();
    for(int v=0;v&lt;V;v++) {
        if(!used[v]) dfs(v);
    }
    memset(used,0,sizeof used);
    int k = 0;
    for(int i=vs.size()-1;i&gt;=0;i--) {
        if(!used[vs[i]]) rdfs(vs[i],k++);
    }
    return k;
}

int n,a,b;
int ar[100100];
map&lt;int,int&gt; ma;
set&lt;int&gt; se;

int main()
{
    ios::sync_with_stdio(false);
    cin&gt;&gt;n&gt;&gt;a&gt;&gt;b;
    for(int i=0;i&lt;n;i++) {
        cin&gt;&gt;ar[i];
        ma[ar[i]] = i;
        se.insert(ar[i]);
    }

    V = 2*n;
    for (int i=0;i&lt;n;i++) {
        if(ar[i] != a-ar[i]) {
            if (se.count(a-ar[i])) {
                add_edge(i,ma[a-ar[i]]);
            }
            else {
                add_edge(i,i+n);
            }
        }
        if(ar[i] != b-ar[i]) {
            if (se.count(b-ar[i])) {
                add_edge(i+n,ma[b-ar[i]]+n);
            }
            else {
                add_edge(i+n,i);
            }
        }
    }
    int k = scc();
    for(int i=0;i&lt;n;i++) {
        if (cmp[i] == cmp[i+n]) {
            cout&lt;&lt;"NO\n";
            return 0;
        }
    }
    cout&lt;&lt;"YES\n";
    for (int i=0;i&lt;n;i++) {
        if(cmp[i]&gt;cmp[n+i]) 
            cout&lt;&lt;0;
        else 
            cout&lt;&lt;1;
        if(i!=n-1) cout&lt;&lt;" ";
    }

}</pre>

&nbsp;

比赛结束后，开始检查错误。在test 9那组数据，本应该是全都是1，但是我的代码输出一大堆0和一些1。仔细检查代码各处，发现没有问题，然后我竟然开始去怀疑_挑战程序设计竞赛_这本神书。 书上说：

> x所在的强连通分量的拓扑序在 <span class='MathJax_Preview'>\(\neg{x}\)</span> 所在的强连通分量之后 <span class='MathJax_Preview'>\(\Leftrightarrow\)</span> x为真 

但是，我看[kuangbin](http://www.kuangbin.net/)的模板和网上许多2-SAT的代码都是用染色的方法来找出一组解，所以我当时怀疑_挑战_上所说是错的。染色这种方法来源自赵爽的国家队论文：

> 如果没有产生矛盾，我们就可以把处在同一个强连通分量中的点和边缩成一个点，得到新的有向图G′。然后，我们把G′中的所有弧反向，得到图G′′。
> 
> 现在我们观察G′′。由于已经进行了缩点的操作，因此G′′中一定不存在圈，也就是说，G′′具有拓扑结构。
> 
> 我们把G′′中所有顶点置为“未着色”。按照拓扑顺序重复下面的操作：
> 
> > 1、 选择第一个**未着色**的顶点x 。把x染成**红色**。
    
> > 2、 把所有与x矛盾的顶点 y （如果存在 <span class='MathJax_Preview'>\(b_{j},\neg{b_{j}}\in \hat{B}\)</span> , 且 <span class='MathJax_Preview'>\(b_{j}\)</span> 属于x代表的强连通分量， <span class='MathJax_Preview'>\(\neg{b_{j}}\)</span> 属于y 代表的强连通分量，那么x和y就是互相矛盾的顶点）及其子孙全部全部染成**蓝色**。
    
> > 3、 重复操作 1 和 2，直到不存在未着色的点为止。此时，G′′中被染成红色的点在图G 中对应的顶点集合，就对应着该 2-SAT 的一组解。 

我就参照这kuangbin的模板和网上的[这份代码](http://www.cnblogs.com/vongang/archive/2012/02/16/2353770.html),将原来那份代码改成用染色生成一组解，然后交上去，WA在test 8，= = 何逗

最后，我去看了[官方题解](http://codeforces.com/blog/entry/13896)：

> If we have number x and a-x, they should be in the same set. If <span class='MathJax_Preview'>\(x \in A\)</span> , it's obvious that <span class='MathJax_Preview'>\(a-x \in A\)</span> . The contraposition of it is <span class='MathJax_Preview'>\(a-x \not\in A \Rightarrow x \not\in A\)</span> , that means if <span class='MathJax_Preview'>\(x \in B\)</span> , a-x should in the set B. Same as the number x, b-x.
> 
> So we can use Disjoint Set Union to merge the number that should be in the same set.
> 
> If a-x doesn't exist, x can not be in the set A. If b-x doesn't exist, b can not be in the set B.
> 
> Then check if there are any conflicts among numbers which should be in the same set.
> 
> There are many other solutions to solve this problem based on the fact that x, a-x, b-x should be in the same set, like greedy, BFS and 2-SAT. 

题解用的是并查集，但是这不重要。重要的是我从题解里面发现，我大神书_挑战_没错，错的是我，我加边的时候少加了一条边。 当a-x存在时,不仅应该添加 **x -> a-x** 这条边，还应该添加 **a-x+n -> x+n** 这样一个跟刚刚那条边形成逆反的一条边。b-x存在的情况同理。
  
我原来的代码加上这两行就AC了：

<pre class="lang:c++ decode:true ">#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;cstring&gt;
#include &lt;map&gt;
#include &lt;set&gt;
#include &lt;cstdio&gt;
using namespace std;

const int MAX_V = 200500;
int V;
vector&lt;int&gt; G[MAX_V],rG[MAX_V];
vector&lt;int&gt; vs;
bool used[MAX_V];
int cmp[MAX_V];

void add_edge(int f,int t)
{
    G[f].push_back(t);
    rG[t].push_back(f);
}

void dfs(int v)
{
    used[v] = true;
    for(int i=G[v].size()-1;i&gt;=0;i--) {
        if(!used[G[v][i]]) dfs(G[v][i]);
    }
    vs.push_back(v);
}

void rdfs(int v,int k)
{
    used[v] = true;
    cmp[v] = k;
    for(int i=rG[v].size()-1;i&gt;=0;i--) {
        if(!used[rG[v][i]]) rdfs(rG[v][i],k);
    }
}

// set V , clear G and rG, add edges, then scc 
int scc()
{
    memset(used,0,sizeof used);
    vs.clear();
    for(int v=0;v&lt;V;v++) {
        if(!used[v]) dfs(v);
    }
    memset(used,0,sizeof used);
    int k = 0;
    for(int i=vs.size()-1;i&gt;=0;i--) {
        if(!used[vs[i]]) rdfs(vs[i],k++);
    }
    return k;
}

int n,a,b;
int ar[100100];
map&lt;int,int&gt; ma;
set&lt;int&gt; se;

int main()
{
    ios::sync_with_stdio(false);
    cin&gt;&gt;n&gt;&gt;a&gt;&gt;b;
    for(int i=0;i&lt;n;i++) {
        cin&gt;&gt;ar[i];
        ma[ar[i]] = i;
        se.insert(ar[i]);
    }

    V = 2*n;
    for (int i=0;i&lt;n;i++) {
        if(ar[i] != a-ar[i]) {
            if (se.count(a-ar[i])) {
                add_edge(i,ma[a-ar[i]]);
                add_edge(ma[a-ar[i]]+n,i+n);
            }
            else {
                add_edge(i,i+n);
            }
        }
        if(ar[i] != b-ar[i]) {
            if (se.count(b-ar[i])) {
                add_edge(i+n,ma[b-ar[i]]+n);
                add_edge(ma[b-ar[i]],i);
            }
            else {
                add_edge(i+n,i);
            }
        }
    }
    int k = scc();
    for(int i=0;i&lt;n;i++) {
        if (cmp[i] == cmp[i+n]) {
            cout&lt;&lt;"NO\n";
            return 0;
        }
    }
    cout&lt;&lt;"YES\n";
    for (int i=0;i&lt;n;i++) {
        if(cmp[i]&gt;cmp[n+i]) 
            cout&lt;&lt;0;
        else 
            cout&lt;&lt;1;
        if(i!=n-1) cout&lt;&lt;" ";
    }

}</pre>

&nbsp;

后来，我自己总结了一下（也不知道对不对），2-SAT加边的时候除了A = 1： **x -> x+n**这类一元的情况，对于二元的情况（二元是指有两个变量A与B，比如 <span class='MathJax_Preview'>\( A\vee B = 1\)</span> ， <span class='MathJax_Preview'>\( A\land B = 0 \)</span> ) 加边的时候要成对的加，每对两条边互为逆反，就比如_挑战_上举的那个例子：

> 子句 <span class='MathJax_Preview'>\( a\vee b \)</span> 改写成等价形式 <span class='MathJax_Preview'>\((\neg a \Rightarrow b \vee \neg b \Rightarrow a)\)</span> 。 

<span class='MathJax_Preview'>\( \neg a \Rightarrow b\)</span> 就与 <span class='MathJax_Preview'>\(\neg b \Rightarrow a\)</span> 互为逆反。这道题里要添加这对边 **x -> a-x** 和 **a-x+n -> x+n**也互为逆反。
  
所以，个人见解：

> 2-SAT加边的时候对于二元的情况要成对的加，每对两条边互为逆反。