---
id: 117
title: Codeforces Round 277
date: 2014-11-16T00:01:05+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=117
permalink: /acm/codeforces-round-277/
duoshuo_thread_id:
  - 1340357450817077254
categories:
  - ACM
tags:
  - ACM
  - CodeForces
---
* * *

### [B. OR in Matrix](http://codeforces.com/problemset/problem/486/B)

&nbsp;

题意：A、B均为0、1矩阵，A矩阵可以通过运算 <span class='MathJax_Preview'>\({B}_{ij}={A}_{i1}OR{A}_{i2}OR...OR{A}_{in}OR{A}_{1j}OR{A}_{2j}OR...OR{A}_{mj}\)</span> 得到B矩阵。现在给出B矩阵，问能否求出A矩阵。

我是先判断是否B矩阵是否合法，然后想办法生成A矩阵。但是codeforces上题解运用逆向思路，根据B矩阵中0的位置，消除A矩阵中的1，然后得到一个A矩阵，然后判断这个A矩阵能否正确生成B矩阵。

我的代码如下：

<pre class="lang:default decode:true">#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;cstring&gt;
using namespace std;

bool ar[110][110];
int nt[110],mt[110];

int main()
{
    int m,n;
    cin&gt;&gt;m&gt;&gt;n;
    for(int i=0;i&lt;m;i++) {
        for (int j=0;j&lt;n;j++) {
            cin&gt;&gt;ar[i][j];
            if(ar[i][j]) 
                mt[i]++,nt[j]++;
        }
    }
    for(int i=0;i&lt;m;i++) {
        for (int j=0;j&lt;n;j++) {
            if(ar[i][j]&&mt[i] != n && nt[j] != m) {
                cout&lt;&lt;"NO";
                return 0;
            }
        }
    }
    vector&lt;int&gt; vm,vn;
    for(int i=0;i&lt;m;i++) 
        if(mt[i] == n)
            vm.push_back(i);
    for(int j=0;j&lt;n;j++) 
        if(nt[j] == m)
            vn.push_back(j);

    if((vm.size()==0&&vn.size()!=0) || (vm.size()!=0&&vn.size()==0)){
        cout&lt;&lt;"NO";
        return 0;
    }

    memset(ar,0,sizeof ar);
    for(int i=vm.size()-1;i&gt;=0;i--) {
        for (int j=vn.size()-1;j&gt;=0;j--) {
            ar[vm[i]][vn[j]] = 1;
        }
    }
    cout&lt;&lt;"YES\n";
    for(int i=0;i&lt;m;i++) {
        for (int j=0;j&lt;n-1;j++) {
            cout&lt;&lt;ar[i][j]&lt;&lt;" ";
        }
        cout&lt;&lt;ar[i][n-1]&lt;&lt;"\n";
    }

}</pre>

&nbsp;

&nbsp;

* * *

### [C. Palindrome Transformation](http://codeforces.com/problemset/problem/486/C)

&nbsp;

这场CF时间略坑，我因为当成11:30开始，没注册上，所以用了队友的号开黑，结果C题代码写出来，他说我的代码太诡异。。

<pre class="lang:default decode:true ">#include &lt;iostream&gt;
#include &lt;string&gt;
#include &lt;map&gt;
using namespace std;

int n,p;
string str;
map&lt;int,int&gt; ma;
bool ar[100100];
bool ar2[100100];

int main()
{
	cin&gt;&gt;n&gt;&gt;p;
	cin&gt;&gt;str;
	int ans = 0;
	int l = 0, r = n-1;
	while(l&lt;=r) {
		if(str[l] != str[r]) {
			ans += min((str[l]-str[r]+26)%26,(str[r]-str[l]+26)%26);
			ma[l]=r;
			ma[r]=l;
			ar[l] = ar[r] = 1;
			ar2[l] = ar2[r] = 1;
		}
		l++;
		r--;
	}
	//cout&lt;&lt;ans&lt;&lt;endl;
	if(ar[p-1]) {
		ar[p-1] = 0;
		ar[ma[p-1]] = 0;
		ar2[p-1] = 0;
		ar2[ma[p-1]] = 0;
	}

	int llen = 0, rlen = 0;
	for(int i=p;i&lt;n;i++) {
		if(ar[i]) {
			rlen = i-p+1;
			ar[i] = 0;
			ar[ma[i]] = 0;
		}
	}
	for(int i=p-2;i&gt;=0;i--) {
		if(ar[i]) {
			llen = p-1-i;
			ar[i]=0;
			ar[ma[i]] = 0;
		}
	}
	int temp = min(2*rlen+llen,2*llen+rlen);
	// cout&lt;&lt;llen &lt;&lt; " "&lt;&lt;rlen&lt;&lt;endl;
	// cout&lt;&lt;temp&lt;&lt;endl;
	llen = 0, rlen = 0;
	for(int i=p-2;i&gt;=0;i--) {
		if(ar2[i]) {
			llen = p-1-i;
			ar2[i]=0;
			ar2[ma[i]] = 0;
		}
	}
	for(int i=p;i&lt;n;i++) {
		if(ar2[i]) {
			rlen = i-p+1;
			ar2[i] = 0;
			ar2[ma[i]] = 0;
		}
	}
	// cout&lt;&lt;llen &lt;&lt; " "&lt;&lt;rlen&lt;&lt;endl;
	// cout&lt;&lt;2*llen + rlen&lt;&lt;endl;
	cout&lt;&lt;ans+min(temp,min(2*llen+rlen,2*rlen+llen));
}</pre>

&nbsp;

* * *

### [D. Valid Sets](http://codeforces.com/problemset/problem/486/D)

&nbsp;

树状DP，不会，看了题解和标程勉强写出来。

>   * Firstly, we solve the case d =  + ∞. In this case, we can forget all <span class='MathJax_Preview'>\({a}_{i}\)</span> since they doesn't play a role anymore. Consider the tree is rooted at node 1. Let <span class='MathJax_Preview'>\({F}_{i}\)</span> be the number of valid sets contain node i and several other nodes in subtree of i ("several" here means 0 or more). We can easily calculate <span class='MathJax_Preview'>\({F}_{i}\)</span>  through <span class='MathJax_Preview'>\({F}_{j}\)</span>  where j is directed child node of i: <span class='MathJax_Preview'>\({F}_{i}=\prod ({F}_{j}+1)\)</span> . Complexity: O(n).
>   * General case: d ≥ 0. For each node i, we count the number of valid sets contain node i and some other node j such that <span class='MathJax_Preview'>\(a_i\leq a_j\leq a_i+d\)</span> (that means, node i have the smallest value a in the set). How? Start DFS from node i, visit only nodes j such that <span class='MathJax_Preview'>\(a_i\leq a_j\leq a_i+d\)</span> . Then all nodes have visited form another tree. Just apply case  d =  + ∞ for this new tree. We have to count n times, each time is O(n), so the overall complexity is O( <span class='MathJax_Preview'>\({n}^{2}\)</span> ). (Be careful with duplicate counting)

我的代码如下：

<pre class="lang:default decode:true ">#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;cstring&gt;
using namespace std;

const int MAXN = 2000+10;
const int MOD = 1e9+7;

int ar[MAXN],f[MAXN];
int n,d;
vector&lt;int&gt; adj[MAXN];
bool vis[MAXN];

void dfs(int u,int root)
{
	vis[u] = true;
	f[u] = 1;
	for(int i=adj[u].size()-1;i&gt;=0;i--) {
		int v = adj[u][i];
		if(!vis[v]) {
			if(ar[v]&lt;ar[root] || ar[v]&gt;ar[root]+d) continue;
			if(ar[v]==ar[root] && v&lt;root) continue;
			dfs(v,root);
			f[u] = (long long)f[u]*(f[v]+1)%MOD;
		}
	}
}

int main()
{
	cin&gt;&gt;d&gt;&gt;n;
	for(int i=1;i&lt;=n;i++) cin&gt;&gt;ar[i];
	int u,v;
	for(int i=1;i&lt;n;i++) {
		cin&gt;&gt;u&gt;&gt;v;
		adj[u].push_back(v);
		adj[v].push_back(u);
	}
	int ans = 0;
	for(int i=1;i&lt;=n;i++) {
		memset(vis,0,sizeof vis);
		memset(f,0,sizeof f);
		dfs(i,i);
		ans = (ans+f[i])%MOD;
	}
	cout&lt;&lt;ans;
}</pre>

&nbsp;