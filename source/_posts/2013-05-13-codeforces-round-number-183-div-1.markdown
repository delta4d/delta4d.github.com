---
layout: post
title: "Codeforces Round #183 (Div. 1)"
date: 2013-05-13 00:15
comments: true
categories: ["codeforces"]
---

[A. Lucky Permutation Triple](http://www.codeforces.com/contest/303/problem/A)
-----------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/183/1/A.cpp)

0到n-1有三个排列a,b,c，若a[i] + b[i] = c[i] (mod n)，则称(a,b,c)为lucky permutation triple，给定n，若方案存在输出任意一个，否则输出-1。

因为a[i] + b[i] = c[i] (mod n)则sigma(a[i]) + sigma(b[i]) = sigma(c[i]) (mod n)，即n\*(n-1) = n\*(n-1)/2 (mod n)，那么n\*(n-1)/2 = 0 (mod n)，可知当n为偶数时使不可能的，所以n只能是奇数，我们有恰好可以构造出n为奇数时的方案：

	a	0	1	2	...n-2	n-1		
	c	n-2	n-3	n-4	...0	n-1
	b	calculate me! & prove it is legal

[B. Rectangle Puzzle II](http://www.codeforces.com/contest/303/problem/B)
------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/183/1/B.cpp)

给矩形[0,n]x[0,m]，矩形内有一点P(x,y)，求一个**面积最大的**矩形[x1,x2]x[y1,y2]包含P且(x2-x1)/(y2-y1)=a/b。若有多解，则选择矩形中心距离P最近的，若还有多解则选择序列x1y1x2y2中字典序最小的。

构造。首先使(a,b)=1，则矩形边长为ak,bk，首先满足面积最大的要求则ak\<=n,bk\<=n，求出最大的k，这样矩形边长就确定了，然后就是怎么摆的问题了，即要满足距离最小的要求，尽量使长度为ak的线段中心往x靠就行了，在这个基础上尽量使x1,y1小就可以了。

[C. Minimum Modular](http://www.codeforces.com/contest/303/problem/C)
--------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/183/1/C.cpp)

给一个序列{ai}，找出最小的m使得ai关于m两两不同余，你可以在{ai}中去掉k(\<=4)个数以达到目的。

可知M=max{ai}+1满足要求，即m\<=M。若ai和aj关于m同余，则m\|ai-aj，预处理出所有ai-aj，然后枚举m，看有多少ai-aj整除m，当数目大于k*(k+1)/2时，直接跳出接着测试下一个数。要使集合A={i\|存在j属于A使得m\|ai-aj}的数目尽量小但是造成m\|ai-aj的影响尽可能大，那么只能是下面的情况x1,x2,...xk,x(k+1)，m\|xi-x(i-1)，这样构成的(i,j)对共有1+2+...+k=k(k+1)/2。当通过这个测试时然后暴力检测m就可以了。

[D. Rotatable Number](http://www.codeforces.com/contest/303/problem/D)
---------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/183/1/D.cpp)

给定数字长度n，和数制上限x找出最大的数制b\<x，使得存在一个长度为n的[Cylic Number](http://en.wikipedia.org/wiki/Cyclic_number)。

cyclic number形式为(b^(p-1)-1)/p，p为素数,(b,p)=1，且b为模p的[原根](http://en.wikipedia.org/wiki/Primitive_root_modulo_n)。那么首先n=p-1，即n+1要为素数，然后暴力枚举b直到b为一个模p的原根就可以了。

[E. Random Ranking](http://www.codeforces.com/contest/303/problem/E)
-------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/183/1/E.cpp)

一场比赛选手i的比赛成绩均匀分布在[li,ri]区间，得分越低排名越高，求每个选手i排第j名的概率。

一开始我想预处理出每个选手i排在选手j前面的概率，然后对于每个选手i用p[n][m]表示前n个选手有m个排到i之前的概率，但是后来发现是不对的，因为{rank(x)\<i}和{rank(y)\<i}不是独立的，比如[0,2],[1,3],[2,4]若1排到2之后，则3肯定在2之后，为了处理这个问题，分区间来讨论。预处理出所有的相邻区间，即(a,b)内不存在其他分点li或ri，这样枚举第i个人处于第k个分数区间，p[n][m]表示当前有n个人的分数在这个区间之前，m个人的分数也在这个区间之内的概率，那么第i个人排到第j名的概率为p[n][m]/m，j\>=n+1。

Summary
-------

这场比赛时间很好，CST21:00。这场比赛基本上一直在搞B，一开始思考错方向，后来醒悟过来是构造，但是sample2过不了，看了好长时间还是觉得自己的解更优，后来问了judge才发现面积最大的条件漏掉了，改了之后又wa了三次，有一次还是代码复制的时候出错的，最后过pretest的时候已经只有100来分了。。写B最后太急了，至少后两次wa应该都是可以避免的，冷静啊冷静。后来20min就乱搞C，后来发现果然使乱搞，太急了，连同余最基本的等价条件都不能够想到。C和D虽然都会写，但是复杂度方面还是不太会搞啊，D直接从10^9暴搜啊，原根是怎么分布的啊。啊啊啊，数学弱爆了啊。
