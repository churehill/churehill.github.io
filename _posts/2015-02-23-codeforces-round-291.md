---
id: 263
title: Codeforces Round 291
date: 2015-02-23T23:40:37+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=263
permalink: /acm/codeforces-round-291/
duoshuo_thread_id:
  - 1340357450817077277
categories:
  - ACM
---
##### [C. Watto and Mechanism](http://codeforces.com/contest/514/problem/C)

* * *

一道字符串hash的题目。计算输入的字符串的hash值，放入unordered_set（需要C++ 11支持），或者放入一个数组里面然后排序，查询的时候用二分查找。查询的时候，改变查询字符串的每一个字符然后计算hash值，查询是否有改变后的字符串。

<pre class="lang:default decode:true">#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;string&gt;
#include &lt;map&gt;
#include &lt;set&gt;
#include &lt;algorithm&gt;
#include &lt;unordered_set&gt;
#include &lt;cstdio&gt;
#include &lt;cmath&gt;
using namespace std;

typedef long long ll;

const ll MOD = 1000000000000000003;

unordered_set&lt;ll&gt; st;
ll pw[600100];

ll hcount(string &str)
{
	int len = str.length();
	ll res = 0;
	for(int i=0;i&lt;len;i++) 
		res = (res*3 + str[i]-'a') % MOD;
	return res;
}

int main()
{
	ios::sync_with_stdio(false);
	int n, m;
	cin&gt;&gt;n&gt;&gt;m;
	string str;
	for(int i=0;i&lt;n;i++) {
		cin&gt;&gt;str;
		st.insert(hcount(str));
	}

	pw[0] = 1;
	for (int i = 1; i &lt; 600100; ++i)
		pw[i] = pw[i-1] * 3 % MOD;

	for(int i=0;i&lt;m;i++) {
		cin&gt;&gt;str;
		int len = str.length();
		bool isOK = false;
		ll res = hcount(str);
		for(int j=0;j&lt;len;j++) {
			for(int k=0;k&lt;3;k++) {
				if('a'+k != str[j]) {
					ll nres = (res + ('a'+k-str[j])*pw[len-1-j])%MOD;
					if(nres &lt; 0) nres += MOD;
					if(st.count(nres)) {
						isOK = true;
						break;
					}
				}
			}
			if(isOK)
				break;
		}
		cout&lt;&lt;((isOK)?"YES":"NO")&lt;&lt;endl;
	}
}</pre>

##### [D. R2D2 and Droid Army ](http://codeforces.ru/contest/514/problem/D)

* * *

首先用建线段树方便查询区间最大值，然后使用尺取法，或者用 二分枚举区间长度+每次遍历区间起点 也可以。

<pre class="lang:default decode:true">#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;string&gt;
#include &lt;map&gt;
#include &lt;algorithm&gt;
#include &lt;cstdio&gt;
#include &lt;cmath&gt;
using namespace std;

int n, m, k;
int arr[5][100010*4];
int N;

void init(int n)
{
	N = 1;
	while(N &lt; n) N *= 2;
}

void update(int k, int a, int dat[]) 
{
	k += N - 1; 
	dat[k] = a;
	while (k &gt; 0) {
		k = (k - 1) / 2;
		dat[k] = max(dat[k * 2 + 1], dat[k * 2 + 2]);
	}
}

int query(int a, int b, int k, int l, int r, int dat[]) 
{
	if(r &lt;= a || b &lt;= l) return 0;
	if(a &lt;= l && r &lt;= b) return dat[k];
	else {
		int v1 = query(a, b, k * 2 + 1, l, (l + r) / 2, dat);
		int v2 = query(a, b, k * 2 + 2, (l + r) / 2, r, dat);
		return max(v1, v2);
	}
}

int main()
{
	ios::sync_with_stdio(false);
	cin&gt;&gt;n&gt;&gt;m&gt;&gt;k;
	int tmp;
	init(n);
	for(int i=0; i&lt;n ;i++) {
		for(int j=0;j&lt;m;j++) {
			cin&gt;&gt;tmp;
			update(i, tmp, arr[j]);
		}
	}
	// cerr&lt;&lt;query(0, 1, 0, 0, N, arr[0])&lt;&lt;endl;
	// cerr&lt;&lt;query(0, 1, 0, 0, N, arr[1])&lt;&lt;endl;

	int l=0, r=1;
	int ans = 0, tl, tr;
	for(; l&lt;n; l++) {
		if(l &gt; r) break;
		for(; r&lt;=n; r++) {
			long long sum = 0;
			// cerr&lt;&lt;l&lt;&lt;" "&lt;&lt;r&lt;&lt;endl;
			for(int j=0; j&lt;m; j++) 
				sum += query(l, r, 0, 0, N, arr[j]);
			// cerr&lt;&lt;sum&lt;&lt;endl;
			if(sum &lt;= k) {
				if(r - l &gt; ans) {
					tr = r, tl = l;
					ans = r - l;					
				}
			}
			else
				break;
		}
	}
	for(int j=0;j&lt;m;j++) {
		if(j) cout&lt;&lt;" ";
		cout&lt;&lt;query(tl, tr, 0, 0, N, arr[j]);
	}
}</pre>

还有一种方法：尺取法 + 堆（或优先队列），思路其实是很奇特的，使用了两组优先队列，topop优先队列存放需要pop出去的元素，而当需要pop出去的元素为当前区间最大值时才真正pop出去。这个方法给我开了眼界，以后数据结构题可能会用到，而且也很好用。

<pre class="lang:default decode:true">#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;string&gt;
#include &lt;map&gt;
#include &lt;algorithm&gt;
#include &lt;queue&gt;
#include &lt;cstdio&gt;
#include &lt;cmath&gt;
using namespace std;

int n,m,k;
int arr[5][100010];
priority_queue&lt;int&gt; qu[5], topop[5];
int tar[5], ans = 0;

int main()
{
	ios::sync_with_stdio(false);
	cin&gt;&gt;n&gt;&gt;m&gt;&gt;k;
	for(int i=0; i&lt;n; i++) 
		for(int j=0; j&lt;m; j++)
			cin&gt;&gt;arr[j][i];

	int l=0, r=0;
	for(; r&lt;n; r++) {
		for(int j=0; j&lt;m; j++) 
			qu[j].push(arr[j][r]);

		long long sum = 0;
		for(int j=0; j&lt;m; j++)	
			sum += qu[j].top();

		if(sum &lt;= k) {
			if(r - l + 1 &gt; ans) {
				ans = r - l + 1;
				for(int j=0; j&lt;m; j++) tar[j] = qu[j].top();
			}
		}
		else {
			for(int j=0; j&lt;m; j++) {
				topop[j].push(arr[j][l]);
				while (!topop[j].empty() && !qu[j].empty() && qu[j].top() == topop[j].top())
					qu[j].pop(), topop[j].pop();
			}
			l++;
		}
	}
	for(int j=0; j&lt;m; j++) {
		if(j) cout&lt;&lt;" ";
		cout&lt;&lt;tar[j];
	}

}</pre>

更新：两个堆实现的这种可以pop非最大最小的元素的效果，可以用set, multiset来实现，因为set和multiset是二叉搜索树红黑树实现的，* set.begin()获取最大最小值，至于pop，先find然后就删除迭代器对应的元素。

当然，两个堆还是一个很新奇方便的思路。