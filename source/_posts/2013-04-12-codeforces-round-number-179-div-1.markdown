---
layout: post
title: "Codeforces Round #179 (Div. 1)"
date: 2013-04-12 14:09
comments: true
categories: ["codeforces"]
---

[A. Greg and Array](http://www.codeforces.com/contest/295/problem/A)
-------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/179/1/A.cpp)

一个数组a，有一些系列操作l,r,d，给a[l..r]中的每个数加d，现在有一系列询问x,y，表示执行操作x..y。输出所有询问后的数组。

每个操作l,r,d可以看作a[l]+=d,a[r+1]-=d，于是a[i]的大小即sum[1..i]，询问x,y可以看作操作x,y,1，只不过对应的是操作数组。

[B. Greg and Graph](http://www.codeforces.com/contest/295/problem/B)
-------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/179/1/B.cpp)

给一个500个点的有向图，现在按给定顺序依此去除一个点，每次去点时输出所有点对最短路的和。

采用[Floyd](http://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm)，按照去除点列的逆序一次加一个点。

[C. Greg and Friends](http://www.codeforces.com/contest/295/problem/C)
---------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/179/1/C.cpp)

过河问题，但是人的重量只有50和100两种，人数为50。现在查询最少需要几步使得所有人到达对岸，在最少的情况下有多少中乘船方案。

记录50的人数a，100的人数b，现在是要过对岸还是返程turn，bfs一遍(a,b,turn)，更新最短路，并且计数。

[D. Greg and Caves](http://www.codeforces.com/contest/295/problem/D)
-------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/179/1/D.cpp)

nxm的矩形方阵，存在这么一些行[l..r]，每一行只有两个格子是黑色，存在t，对于[l..t]的行，第i行的以黑色块为端点构成的线段包含在i+1行对应的线段内，[t..r]内刚好相反，i+1包含在i内。求所有染色方案数。

如果以t为分界的话，那么上面是一个类似三角的图形，下面是一个倒三角，所以只需计算形成三角形的方案数。设f(i,j)表示三角形第i层长度为j的方案数，那么第i-1层的长度可以有2..j种，i-1层长度为k包含在长度为j的线段上的方案数为j-k+1，于是可以得到递推关系:

$$
f(i,j)=\sum_{k=2}^{j}f(i-1,k)*(j-k+1)
$$

化简得到：

$$
f(i,j)=2f(i,j-1)-f(i,j-2)+f(i-1,j)
$$

接下来就可以枚举t，和第t行的长度k。这里t需要做一个确切的定义，取最后一个使[l..t]“递增”的t，即t+1相对于t是严格减小的。于是对应一个(t,k)枚举上三角的高度和下三角的高度就可以了。对于一个高度为h，最后一行长度为k，倒数第二行严格小与最后一行的方案数为f(h,k)-f(h,k-1)。

那么上三角总数为

$$
A(t,k)=\sum_{i=0}^{k}f(1,i)
$$

下三角总数为

$$
B(t,k)=\sum_{i=2}^{n-t+1}(f(i,k)-f(i-1,k))+f(1,k)=f(n-t+1,k)
$$

第t行长度为k共有m-k+1排法，于是最后的方案总数为：

$$
T(n,m)=\sum_{t=1}^{n}\sum_{k=2}^{m}(A(t,k)*B(t,k)*(m-k+1))
$$

[E. Yaroslav and Points](http://www.codeforces.com/contest/295/problem/E)
------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/179/1/E.cpp)

有一个数列{xi}，现在有一个操作i,pi,di，表示x[pi]=x[pi]+di，有一系列查询l，r表示查询位于区间[l,r]内的xi的两两差的和，即$$\sum_{l\le x_i\le xj\le r}(x_j-x_i)$$。

具体做法是线段树，所查询的和可以由两个部分合并，每个区段需要保留三个参数，该区间内数的多少，数的和，和该区间内的两两差的和。

Summary
-------

A比较顺利，B一开始将500看成5000了，想不出来，后来仔细一看发现是500，赶紧按floyd写了一遍，提交wa，后来又wa了两次，在还剩30min的时候过掉，当时没有想清楚就直接写然后交了，以后一定要先想清楚，否则真是得不偿失啊。后来看C，虽然可以做，但是实在没有20min写对的实力，如果B过的快一点，C还是有可能的。很不扎实啊，尤其是B的floyd。啊啊啊，弱爆了弱爆了。
