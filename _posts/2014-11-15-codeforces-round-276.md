---
id: 69
title: Codeforces Round 276
date: 2014-11-15T21:35:37+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=69
permalink: /acm/codeforces-round-276/
duoshuo_thread_id:
  - 1340357450817077253
categories:
  - ACM
tags:
  - ACM
  - CodeForces
---
* * *

### [C. Bits](http://codeforces.com/contest/485/problem/C)

题意：给出两个数l和r, 求l与r之间含最多1的数

将原来的[l , r]中的r加1，区间变为[l , r)。计算出l和r的位数lc, rc，然后分两种情况讨论，如果rc>lc，则直接选比r小且离r最近的 <span class='MathJax_Preview'>\({2}^{b}-1\)</span> 为结果，b为正整数，可以证明这个数一定大于等于l；如果rc == lc，则从高位一直遍历l和r的每位数字，直到找到某位的r的数字大于l的数字（也就是r的该位为1，l的该位为0），将这一位改为0，这一位后面的数位用1填充就可以得到目标数字。

我的代码如下：

<pre class="lang:c++ decode:true">#include &lt;iostream&gt;
using namespace std;

int la[100],ra[100];
 
int main()
{
	int n;
	cin&gt;&gt;n;
	long long l,r;
	while(n--) {
		cin&gt;&gt;l&gt;&gt;r;

		r ++;
		int lc=0,rc=0;
		while(l&gt;0) {
			la[lc++] = l%2;
			l /= 2;
		}
		while(r&gt;0) {
			ra[rc++] = r%2;
			r /=  2;
		}
		if(rc &gt; lc){
			cout &lt;&lt; (1LL&lt;&lt;(rc-1))-1 &lt;&lt;"\n";
		}
		else {
			int i ;
			long long ans = 0;

			for(i=rc-1;i&gt;=0;i--) {
				ans *= 2;
				if (ra[i]&gt;la[i])
					break;
				else {
					ans += ra[i];					
				}
			}
			cout &lt;&lt; (1LL&lt;&lt;i)-1 + (ans &lt;&lt; i)&lt;&lt;"\n";
		}
	}
}</pre>

&nbsp;

* * *

### [D. Maximum Value](http://codeforces.com/contest/485/problem/D)

&nbsp;

哎，没做出，当时一直往三分上想，然后就各种WA了。。

看了codeforces的题解后，有些懂了，题解如下：

> Let us iterate over all different <span class='MathJax_Preview'>\({a}_{i}\)</span> . Since we need to maximize <span class='MathJax_Preview'>\({a}_{i}%{a}_{j}\)</span> , then iterate all integer x (such x divisible by <span class='MathJax_Preview'>\({a}_{j}\)</span> ) in range from <span class='MathJax_Preview'>\(2{a}_{j}\)</span>  to M, where M — doubled maximum value of the sequence. For each such x we need to find maximum <span class='MathJax_Preview'>\({a}_{j}\)</span> , such <span class='MathJax_Preview'>\({a}_{j}\)</span>  < x. Limits for numbers allow to do this in time O(1) with an array. After that, update answer by value <span class='MathJax_Preview'>\({a}_{i}%{a}_{j}\)</span> . Total time complexity is O(nlogn + MlogM).

&nbsp;

其实M不需要是序列最大值的2倍，只需要是比序列最大值大的一个 <span class='MathJax_Preview'>\({a}_{i}\)</span> 的倍数即可。

代码如下：

<pre class="lang:default decode:true ">#include &lt;iostream&gt;
#include &lt;algorithm&gt;
using namespace std;

int n;
int ar[200100];

int main()
{
	ios::sync_with_stdio(false);
	cin&gt;&gt;n;
	for (int i=0;i&lt;n;i++) 
		cin&gt;&gt;ar[i];
	sort(ar,ar+n);

	int ans = 0;
	for (int i=0;i&lt;n;i++ ) {
		if(i&gt;0 && ar[i]==ar[i-1])
			continue;
		int lb = ar[i]*2;
		int ub = (ar[n-1]/ar[i]+1)*ar[i];
		for(int b=lb;b&lt;=ub;b+=ar[i]) {
			int pt = lower_bound(ar+i+1,ar+n,b)-ar;
			ans = max(ans,ar[pt-1]%ar[i]);
		}

	}
	cout&lt;&lt;ans;
}
</pre>

&nbsp;