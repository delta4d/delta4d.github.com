---
layout: post
title: "单调队列/栈"
date: 2013-04-06 17:43
comments: true
categories: ["algorithm"]
---

这两天连续碰到单调队列/栈的问题，稍微总结一下。

单调队列/栈即在队列/栈的基础上加上单调。其中每个元素进出各一次，所以复杂度一般是O(n)。单调队列实际上是一个双端队列，队列头删除元素，队列尾添加元素。比较典型的应用是k窗口问题，即一个数列{xi}，用一个长度为k的窗口从左向右扫描，每次都查询当前窗口中的最大元素。

采用单调递减队列，即队列头总是最大元，有新的元素加入时，总是从队尾开始扫描，直到遇到第一个比自己大的，然后插入队尾。窗口每移动一格，从队头开始去除旧元素。可以看出，单调队列里不仅值是单调的，索引也是单调的，即队尾总是最新的。

这个可以用于一类如f[x]=max{g(k)\|b[x]\<=k\<x}+w[x]的问题，其中b[x]不减。如果j\<k，并且g(j)\<g(k)，那么g(j)就可以被舍弃，因为b[x]单调不减，而g(j)又没有g(k)优，它已经不可能再影响以后的状态了。感觉有些长江后浪推前浪，前浪死在沙滩上的感觉。。比如k窗口问题中k=3，x={3,5,6,2,7}，扫描到6时，比5优，于是便可以舍弃5了，因为以后包含5的窗口肯定会包含6，而5又没有6优。

单调栈感觉就是没有删除操作的单调队列，比较典型的是[ZOJ 1985 Largest Rectangle in a Histogram](http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemId=985)，用于求每个元素两侧第一个比自己大的。

下面是这两天在[Codeforces](http://www.codeforces.com)中遇到的一些题目。

[Maximum Xor Secondary](http://www.codeforces.com/contest/280/problem/B)
-----------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/172/1/B.cpp)

有一个数列{xi}到整数的映射f：{xi}中第一大和第二大元素的异或值。现在给定数列{xi}，求max{f(x[L..R])}。

直接求的话，只枚举所有子列就需要n^2，复杂度太高，然而可以发现有很多区间是不用判断的。可以采用单调递增栈，即栈顶总是当前最小的（不是所有元素里最小的），设栈为stack[sz]，那么stack[sz-1]为左边第一个比stack[sz]大的元素，于是只需要计算stack[sz-1]^stack[sz]。当有新的元素x，只需要不断扫描栈，直到遇到第一个比x的元素即可。

[Bindian Signalizing](http://www.codeforces.com/contest/5/problem/E)
---------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/005/E-monotonous-stack.cpp)

一个圆上有一圈数{xi}，一个合法的数对(i,j)为在至少有一条连接i，j的弧中的所有数都不大于xi和xj，求合法数对的个数。

首先将最大元循环移动放到第一个元素，这样就基本将圈变成了链。单调递增栈，栈里维护了最左端第一个比当前元素大的元素。在更新的时候pop了几次，最后结果就要加几次。比如5 1 4 1 3 6，当前元素为2，首先6找到3，然后比3大的第一个元素为4，比4大的第一个为5，于是(5,6)(4,6)(3,6)都是合法的。具体处理的时候还要注意元素相等的情况和逆向可以到达第一个元素的情况。

[Exposition](http://www.codeforces.com/contest/6/problem/E)
------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/006/2/E.cpp)

有一个数列{xi}，设映射f: array x -\> y=max{x}-min{x}，求max{L-R\|f(x[L..R])\<=k}。

类似与k窗口，需要维护一个区段的最大值和最小值，采用两个单调队列，一个维护最大值，一个维护最小值。
