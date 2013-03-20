---
layout: post
title: "Codeforces Round #174 (Div. 1)"
date: 2013-03-18 21:00
comments: true
categories: ["codeforces"]
---

[A. Cows and Sequence](http://www.codeforces.com/contest/283/problem/A)
----------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/174/1/A.cpp)

对于一个数列，有三个操作：

	1. 给a个数同时加x
	2. 在末尾加入元素k
	3. 去掉末尾的元素

对于每一次操作，查询数列的平均值，共有2\*10^5次操作。三个操作都可以看作区间操作，[1,a]上同时加x，[sz,sz+1]加x，[sz-1,sz]减去数列末尾sz处的值，于是可以用树状数组来做。 

[B. Cow Program](http://www.codeforces.com/contest/283/problem/B)
----------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/174/1/B.cpp)

初始时给定两个数x=1，y=0，还有一个序列{an}，在这个基础上有一些列操作

	1. 某一步后若x<=0或y>n，程序终止
	2. x+=ax, y+=ax
	3. x-=ax, y+=ax
	4. 交替执行步骤2和3，或许有限步后结束，或许永远不会终止

现在给定序列{a[2..n]}，接下来将这个程序跑n-1遍，第i遍执行序列i, a2, ..., an，若程序终止，输出y的值，否则输出-1

*注意2和3中的ax，其中是以x是变量，是数组下标。比赛时没仔细看直接以为按顺序执行ai，做了很多无用功。略怨念*

可以看到共有n\*2个状态，2代表当前操作是2还是3，可以直接记忆化搜索。我写的比较麻烦，用了map\<int, vector\<int\> \>来表示图，比如mp[a+ax] = x。

[C. Coin Troubles](http://www.codeforces.com/contest/283/problem/C)
------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/174/1/C.cpp)

给定n种硬币，面值总共为t，然后有q种限制(bi, ci)，表示类型bi的硬币比ci多，现在要求有多少种方案。其中bi各不相同，ci各不相同。

对于每种限制(bi,ci)，从bi到ci连一条有向边，所谓bi各不相同就是同一个点不能有1条以上出边，ci各不相同即一个点不能有1条以上入边，于是只能够构成一条链，也有可能是圈，然而这是不合法的，直接输出0即可。找到链后，比如是x1,x2,...,xk，于是可以做新的硬币，Si=x1+...+xi，就可以处理，xi一定要比xj多的要求(i>j)，找方案数的话做一遍背包就可以了。其实细节还是挺多的，具体可以看代码。


[D. Cows and Cool Sequences](http://www.codeforces.com/contest/283/problem/D)
----------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/174/1/D.cpp)

定义cool pair (x,y)是x能够用y个连续整数的和表示，cool sequence{an}是对所有(ai,a{i+1})是cool pair。想在给定一个数列，要求改动最少的数使其是cool sequence。还是直接看[官方报告](http://www.codeforces.com/blog/entry/7036)吧，基于一个发现然后n^2dp。


[E. Cow Tennis Tournament](http://www.codeforces.com/contest/283/problem/E)
--------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/174/1/E.cpp)

数列{sn}表示一些cow的等级，cow\_i能够打败cow\_j当且仅当s\_i>s\_j，现在有一些操作[a,b]，等级在在区间[a,b]里的cow的胜负关系会发生改变。最后问查询无序数对(p,q,r)有多少，其中存在一个排列使得cow\_p->cow\_q->cow\_r->cow\_p，i->j表是cow\_i可以击败cow\_j。

首先最后答案为C(m,3)-sigma(C(win\_i,2))。用c[n][n]表示胜负关系是否被改变，那么当一次查询[a,b]时，矩形[a,b]x[a,b]里的关系将被反转，用垂直扫描线，线段树就可以解决。具体还是看[官方报告](http://www.codeforces.com/blog/entry/7036)吧。

Summary
-------

A，B，C都是可以做的，但是比赛的时候只搞出了A，B和C在赛后很快fix了，很不扎实啊(>\_<)。D和E只能通过看官方报告来获取想法，希望能一点一点提高吧。总之这是一套很不错的题目。以上。。
