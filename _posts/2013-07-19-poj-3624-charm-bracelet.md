---
id: 168
title: POJ 3624 Charm Bracelet
date: 2013-07-19T14:59:24+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=168
permalink: /acm/poj-3624-charm-bracelet/
duoshuo_thread_id:
  - 1340357450817077260
categories:
  - ACM
tags:
  - POJ
  - 背包问题，DP
---
<div lang="en-US">
  这一题是0-1背包问题，属于每个物品只能用一次的那种，这类问题关键的是找出状态转移方程:
</div>

<div lang="en-US">
  dp[i][j] =  max(dp[i-1][j],dp[i-1][j-v[i]]+w[i]), 其中i表示物品的编号，j表示背包的容量，dp[i][j]就表示用前i个物品装容量为j的背包时能达到的最大的质量，v[i]表示第i个物品的体积，w[i]表示第i个物品的重量。这个状态转移方程大意就是计算到第i个物品时，有两种选择：一种是不装入该物品，那就是表示背包容量j是由前i-1个物品填满的, 表达式是dp[i-1][j]，另一种是装入了该物品，前i-1个物品占据了dp[i-1][j-w[i]]容量，所以表达式是dp[i-1][j-v[i]]+w[i]，所以为了取得最大重量，应该找这两种方案的最大值，即dp[i][j] =  max(dp[i-1][j],dp[i-1][j-v[i]]+w[i])。
</div>

<div lang="en-US">
  就POJ这一题来说，状态转移方程应该为d[i][j]] = max(d[i-1][j],d[i-1][j-w]+d),未经滚动数组优化的代码如下（提交上去会MLE的）：
</div>

<div lang="en-US">
  <pre class="lang:c++ decode:true ">#include &lt;iostream&gt;
#include &lt;cstdio&gt;
#include &lt;fstream&gt;
using namespace std;
const int maxm = 12881;
const int maxn = 3403;
int dp[maxn][maxm];

int main()
{
    freopen("in.txt","r",stdin);
    int n,m,w,d;
    scanf("%d%d",&n,&m);
    for(int i=1;i&lt;=n;i++)
    {
        scanf("%d%d",&w,&d);
        for(int j=m;j&gt;0;j--)
        {
            if(j&gt;=w)
                dp[i][j]=max(dp[i-1][j],dp[i-1][j-w]+d);
            else
                dp[i][j]=dp[i-1][j];
        }
    }
    printf("%d\n", dp[n][m]);
}</pre>
  
  <p>
    &nbsp;
  </p>
  
  <div class="ptx" lang="en-US">
    上面的代码会MLE，这一题很抠内存，所以应该用滚动数组优化一下。
  </div>
  
  <div class="ptx" lang="en-US">
    滚动数组将上面的dp[][]数组转化为一维的，由于DP算法的无后效性：父问题的解只依赖于直接子问题的解，子问题的解只影响父问题的解。换句话说，孙子是影响不到爷爷的(爷爷的解的计算不需要孙子的解)
  </div>
  
  <div class="ptx" lang="en-US">
    所以可以只保存计算中的父与子，但是滚动数组更加优化，只用一维的数组，代码如下：
  </div>
  
  <div class="ptx" lang="en-US">
    <pre class="lang:c++ decode:true ">#include &lt;iostream&gt;
#include &lt;cstdio&gt;
#include &lt;fstream&gt;
using namespace std;
const int maxm = 12881;
int dp[maxm];
int main()
{
    freopen("in.txt","r",stdin);
  int n,m;
  int w,d;
  scanf("%d%d",&n,&m);
  for(int i=1;i&lt;=n;i++)
  {
    scanf("%d%d",&w,&d);
    for(int j=m;j&gt;=w;j--)
      dp[j]=max(dp[j-w]+d,dp[j]);
  }
  printf("%d\n", dp[m]);
}</pre>
    
    <p>
      &nbsp;
    </p>
    
    <div class="ptx" lang="en-US">
      这里的dp数组时<strong>从上到下、从右到左</strong>计算的。在计算dp(i,j)之前，dp[j]保存的就是dp(i-1,j)的值，而dp[j-w]里保存的是dp(i-1,j-w)。这样，dp[j]=max(dp[j-w]+d,dp[j])实际上是把max(dp(i-1,j),dp(i-1,j-w)+d)保存在dp[j]中，覆盖掉dp[j]原来的dp(i-1,j)。
    </div>
    
    <p>
      &nbsp;
    </p>
  </div>
</div>