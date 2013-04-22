---
layout: post
title: "Codeforces Round #180 (Div. 1)"
date: 2013-04-20 17:56
comments: true
categories: ["codeforces"]
---

[A. Parity Game](http://www.codeforces.com/contest/297/problem/A)
----------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/180/1/A.cpp)

有两个01串a和b，对于a有两种操作，a-\>a[1..-1]和a-\>a+parity(a)，问能否通过这两种操作使a变为b。

考察操作中的不变量，可以看到，a中1的个数是确定的，如果a中1的个数为偶数，那么两种操作均不会增加1，如果为奇数，那么1的个数之多智能增加1，对于parity操作0是可以随意添加的，于是只要a中最多能有的1的个数不比b少，那么a均可以变为b。

[B. Fish Weight](http://www.codeforces.com/contest/297/problem/B)
----------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/180/1/B.cpp)

去掉背景，就是问对于给定的$$\sum_{i=1}^{k}b_iw_i$$和$$\sum_{i=1}^{k}c_iw_i$$，$$0<w_1\le w_2\le ...\le w_n$$，能否有{wi}的一个分配方案使得使第一个式子严格大于第二个。

首先用第一个式子减第二个，令$$a_i=b_i-c_i$$，得到$$X=\sum_{i=1}^{k}a_iw_i$$，问X是否可以严格大于0。可以发现，如果an>0，那么使wn趋于无穷大，总可以使X\>0，那么当an<0时，我们肯定要使anwn造成的影响最小，那么要使wn最小，根据条件可以知道，wn最小为w[n-1]，于是w[n-1]系数变为a[n]+a[n-1]，规模化为n-1的原问题，如此一直向前，如果存在某个系数\>0，那么可知分配方案是存在的，否则无解。

[C. Splitting the Uniqueness](http://www.codeforces.com/contest/297/problem/C)
-----------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/180/1/C.cpp)

有一个数列{si}，对于每个si可以分解为si=ai+bi，于是由{si}可以生成两个数列{ai},{bi}。现在定义almost unique的数列为之多去掉ceil(n/3)项可以使数列项两两不同的数列，问是否存在si的一个分法，使得{ai}{bi}almost unique。其中si,ai,bi\>=0，且si两两不同。

不妨设s1<s2<...<sn。假设一个数列{xn}中不同元素的个数为k，于是要使{x}unique，至少要去掉n-k个元素，于是almost unique的数列中不同元素的个数至少为n-ceil[n/3]。如果{si}分布很稀疏的话，比如s[i+1]-s[i]>=2，那么只要让a[i]=i,b[i]=s[i]-i就可以了，因为s[i]各不相等，所以b[i+1]\>[i]，于是只要集中精力考虑{si}很密集的情况就可以了，比如{si}={1,2,...,n}。可以有以下的分法：

	s: 1 2 3 4 5 6 7 8 9	1 2 3 4 5 6 7 8 9
	a: 1 2 3 0 0 0 4 5 6 	1 0 3 1 4 2 5 3 6
	b: 0 0 0 4 5 6 1 2 3	0 2 0 3 1 4 2 5 3

两种分法都是可以的。

[D. Color the Carpet](http://www.codeforces.com/contest/297/problem/D)
---------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/180/1/D.cpp)

给一个h\*w的方格，然后用k种颜色给这个方格染色，给出相邻单元格颜色之间的关系(相同和不相同)，问是否存在一个染色方案使得至少满足3/4的颜色关系。

首先共有T=w(h-1)+h(w-1)个约束关系。假若k=1，那么只有一种染色方案，直接判断就可以了，如果k\>1，接下来说明这种方案总是存在的。只需构造出k=2的方案。首先将第一行染色，满足第一行的行关系，然后染第二行，使得满足第二行的行关系，现在比较第一行和第二行之间不满足的列关系个数，记为c，如果c<=w/2，那么继续，否则交换第二行的两种颜色，可知第二行的行关系依然满足，但是列关系不满足的个数变为w-c<=w/2，如此继续直到最后一行。那么不满足的关系至多为S=w/2\*(h-1)，如果h\>=w，则可知S/T<=3/4，如果h<w，那么先染列就可以了。


[E. Mystic Carvings](http://www.codeforces.com/contest/297/problem/E)
--------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/180/1/E.cpp)

略去题目背景，最后问题转化为要高效查询一个区间内有多少个完整区间，并且区间是在环上的。

首先考虑一条直线的情形，假设区间集合为{ai,bi}，按bi递增排序，那么当处理到第k个区间[ak,bk]时，前面的所有区间[ai,bi]都会有bi\<bk，于是只需查询有多少ai\>ak，可以用树状数组或线段树来记录ai，每处理完一个区间后在ai处加1就可以了。

然后考虑环的情况，将整个大区间扩成两倍长即可，于是以前的区间[a,b]可以变为[b,a+n],[a+n,b+n]，可以看到对于两个区间[a,b]和[x,y]，[x,y]只能包含于[a,b]和[b,a+n]中的一个，于是环的情况也解决了。

题目具体的分解转化可以看官方[Tutorial](http://www.codeforces.com/blog/entry/7440)。

Summary
-------

A完全考虑错了方向，但是居然过了pretest，pretest也太弱了，B比较顺利，C比较侥幸。这应该是典型的需要"Do More Thinking Than Coding"的比赛，差不多都需要去充分挖掘题目条件给特定问题带来哪些特点和便利，观察啊观察，从反面考虑啊，神奇的构造啊，啊啊啊，弱爆了弱爆了。
