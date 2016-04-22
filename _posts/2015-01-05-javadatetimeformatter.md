---
id: 152
title: Java日期格式中的的陷阱
date: 2015-01-05T13:18:48+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=152
permalink: /tech/javadatetimeformatter/
duoshuo_thread_id:
  - 1340357450817077256
categories:
  - Tech
tags:
  - Date
  - DateTimeFormatter
  - java
---
今天在InfoQ上看到了这样的一片文章<a title="InfoQ" href="http://www.infoq.com/cn/news/2015/01/java-date-format-with-caution" target="_blank">《慎用Java日期格式化》</a>，是关于Java中DateTimeFormatter的yyyy与YYYY的区别，现行的公历的正确格式化显示方法是"yyyy-MM-dd"，而不是"YYYY-MM-dd"。

文章中写了格里高利历（现行公历）与ISO 8601日历的区别，ISO 8601规定**一年中第一个周四所在的那个星期，作为这一年的第一个星期**。

> 那么日期为什么忽然变得不对了？原因是开发人员误用的格式符代表的是一种不同的日历系统。现行的公历通常被称为格里高利历（Gregorian calendar），它以400年为一个周期，在这个周期中，一共有97个闰日，在这种历法的设计中，闰日尽可能均匀地分布在各个年份中，所以一年的长度有两种可能：365天或366天。而本文提到的被错误使用的历法格式，是国际标准ISO 8601所指定的历法。这种历法采用周来纪日，样子看起来是这样的：2009-W53-7。对于格里高利历中的闰日，它也采用“闰周”来表示，所以一年的长度是364或371天。并且它规定，公历一年中第一个周四所在的那个星期，作为一年的第一个星期。这导致了一些很有意思的结果，公历每年元旦前后的几天，年份会和ISO 8601纪年法差一年。比如，2015年的第一个周四是1月1日，所以1月1日所在的那周，就变成了2015年的第一周。代表ISO 8601的格式符是YYYY，注意是大写的，而格里高利历的格式符是小写的yyyy，如果不小心把这两者搞混了，时间就瞬间推移了一年！维基百科上也有<a title="wikipedia" href="http://en.wikipedia.org/wiki/ISO_week_date" target="_blank">词条</a>专门解释ISO 8601。
> 
> &nbsp;

我刚看到这段描述的时候自己试了一下，用的new Date()，发现没什么问题，都是输出2015-1-5。。

然后我仔细看了文章的来源<a href="https://twitter.com/jmhodges/status/549430032616017921" target="_blank">Twitter</a>，发现这个Twitter是在2014年12月28日发出来的，用2014-12-28再试了一下，YYYY-MM-dd果然输出的是2015-12-28。。

[<img class="size-full wp-image-153 aligncenter" src="http://blog.tpircsboy.com/wp-content/uploads/2015/01/date.jpg" alt="date" width="244" height="218" />](http://blog.tpircsboy.com/wp-content/uploads/2015/01/date.jpg)

&nbsp;

原因在于2014-12-28是周日，刚好处于ISO 8601中所说的第一周，所以按ISO 8601，现行公历的2014-12-28已经是2015年了，所以YYYY输出的就是2015，同理现行公历的2014-12-29、30、31也属于ISO 8601的2015年。而现行公历的2015-1-5显然已经是第二周了，所以yyyy-MM-dd与YYYY-MM-dd输出相同。

也就是说如果代码里面用YYYY-MM-dd，在某些年份的第一周都会出现问题。

言不达意，只能贴出代码了。

<pre class="lang:java decode:true ">import java.util.*;
import java.text.SimpleDateFormat;

public class Test {
	public static void main(String[] args) {
		SimpleDateFormat fm1 = new SimpleDateFormat("yyyy-MM-dd");
		SimpleDateFormat fm2 = new SimpleDateFormat("YYYY-MM-dd");

		System.out.println(fm1.format(new Date()));
		System.out.println(fm2.format(new Date()));
		try {
			System.out.println(fm2.format(fm1.parse("2014-12-28"))); // print 2015-12-28  (error)
			System.out.println(fm2.format(fm1.parse("2014-12-29")));  // print 2015-12-29  (error)
			System.out.println(fm2.format(fm1.parse("2014-12-30")));// print 2015-12-30  (error)
			System.out.println(fm2.format(fm1.parse("2015-1-5")));// print 2015-01-05
		}
		catch(Exception e){
		}
	}
}</pre>

但是为什么没人弄混MM跟mm，DD和dd呢，我想原因就在于MM和mm输出结果明显不一样，DD和dd也是，在调试的时候应该能够轻易发现，M表示的是月份month-of-year， m表示的是分钟minute-of-hour，D表示的是一年中的第几天day-of-year，d表示的是一月中的第几天day-of-month。

另附上java官方关于各种日期格式的参考文档 <a href="http://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html" target="_blank">http://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html</a>