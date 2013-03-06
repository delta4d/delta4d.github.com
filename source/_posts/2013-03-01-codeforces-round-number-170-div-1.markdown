---
layout: post
title: "Codeforces Round #170 (Div. 1)"
date: 2013-03-01 15:16
comments: true
categories: ["codeforces"]
---

[A. Learning Languages](http://www.codeforces.com/contest/277/problem/A)
-----------------------
[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/170/1/A.cpp)

n个员工，m种语言，每个员工可以说0种或某些语言，语言相同的员工可以直接沟通，员工也可以间接沟通，即有员工帮他们翻译即可，于是只要是通过语言构成的联通块种任意两个人都可以沟通。问最少还要某些员工学几种语言可以达到员工两两可以沟通。

首先通过dfs或并查集找出所有联通块，可以发现每个联通块所会的语言是不同的，否则他们就联通了，现在只要将所有联通块连起来就可以了，可以看出需要再学联通块个数-1种语言就好了。每个人都不会任何语言的情况要特殊处理下。


[B. Set of Points](http://www.codeforces.com/contest/277/problem/B)
------------------
[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/170/1/B.cpp)

一个平面点集的convexity是指最大的能构成凸多边形的子集元素个数。问n个点，convexity为m是否可行，可行的话输出任意一组可行解，要求任意三点不共线，点为整点。否则输出-1。(m<=n<=2m)

构造性算法。说到凸多边形，很容易联想到圆的内接多边形，在加上n<=2m的限制，便想到两个同心多边形。至于不能的情况，便是m=3，n>=5了。

[C. Game](http://www.codeforces.com/contest/277/problem/C)
---------
[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/170/1/C.cpp)

有一张方格纸，两个人可以轮流用剪刀沿着纵横方向剪，每次不必剪透，可以只剪一部分，但是不能不剪，剪完后的纸张还是在原位。当方格纸变为一个一个单位正方形时游戏就结束了，剪最后一刀的人获胜。现在给出一个已经经过剪裁的纸张，判断先手还是后手赢，若先手赢给出先手的第一步。

类似[NIM](http://en.wikipedia.org/wiki/Nim)。由于行列是分离的，所以经过分离后会形成m-1+n-1个堆，和NIM对应起来。比较复杂的是处理出这些堆，处理完后直接用NIM的结果就可以了。

[D. Google Code Jam](http://www.codeforces.com/contest/277/problem/D)
--------------------
[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/170/1/D.cpp)

模拟codejam，找出最优解题顺序。注意罚时是最后一个正确的提交对应的时间，另外注意精度问题。具体可看[这个](http://www.codeforces.com/blog/entry/6815)

[E. Binary Tree on Plane](http://www.codeforces.com/contest/277/problem/E)
-------------------------
[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/170/1/E.cpp)

平面上有一些点，现在要连一些边，使这些点构成一个有向二叉树，并且线段总和最小。要求一个点的父亲的纵座标要严格大于当前点的纵座标。方案不存在的话输出-1。

最小费用最大流，由纵座标小的向纵座标大的连边，由于有二叉的限制，所以拆分点，使入度最大为2。具体可看代码。
