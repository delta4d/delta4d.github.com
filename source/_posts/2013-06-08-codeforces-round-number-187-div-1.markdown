---
layout: post
title: "Codeforces Round #187 (Div. 1)"
date: 2013-06-08 13:23
comments: true
categories: ["codeforces"]
---

[A. Sereja and Contest](http://codeforces.com/contest/314/problem/A)
-----------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/187/1/A.cpp)

有一列数{an}，定义$$d_i=\sum_{j=1}^{i-1}(a_j(j-1)-(n-i)a_i)$$，现在要求去掉一些ai使得去掉后得到的子列对应的d每一项都不小于给定的k。

直接模拟就可以了，遇到di\<k时，去掉此时的ai，注意去掉i后对于i之前的没有影响，计算这个ai对于之后dj的影响，然后继续扫描就可以了。计算影响时，假设已经去掉了ai1,ai2..,然后按照di的计算公式计算出差值就可以了。

[B. Sereja and Periods](http://codeforces.com/contest/314/problem/B)
-----------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/187/1/B.cpp)

定义字符串s的数乘ns为n个s相连，现在给定ba，dc，其中a，c为字符串，求ba的子串最多包含多少个dc。

贪心，考虑当从a[0],c[i]开始匹配，将a匹配完后下一个匹配到的c的位置和总共匹配的字符长度是多少，然后扫描一遍就可以了。

[C. Sereja and Subsequences](http://codeforces.com/contest/314/problem/C)
----------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/187/1/C.cpp)

给定数列{an}，对于a的所有**不同的**非递减子列{b}，计算sum(product(bi))。

动态规划，用p[i]表示以i结尾的子列的积之和，那么p[j]=j*sum(p[i], i\<=j)+j，表示排在i后面和j单独构成一个子列的情况。由于要计算所有不同的子列，所以当i=j时会有重复，此时要减掉以前的p[j]。

比如1，2，2

	1. p[1] = 1
	2. p[2] = 2 * p[1] + 2 = 4
	3. p[2] = 2 * (p[2] + p[1]) + 2 = 12, 此时有重复所以要减去旧的p[2]=4，即最后结果为12-4=8

将以上三步的结果加起来最后结果为1+4+8=13

[D. Sereja and Straight Lines](http://codeforces.com/contest/314/problem/D)
------------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/187/1/D.cpp)

二维平面上有一堆点，定义点之间的距离为曼哈顿距离，现在画两条正交的直线，其中一条与x轴夹角为45度，现在找出所有点到两条直线的距离较小值里的最大值，现在要求最小化最大值。

因为两条直线斜率为1和-1，根据距离的定义，点到直线的距离便为该点到直线的横向或纵向的距离。设两条直线为y=x+a,y=-x+b，则d(xi,yi)=min(\|yi-xi-a\|,\|yi+xi-b\|)，目标为min(max(d(xi,yi)))，假设对于某种直线的放置d=max(d(xi,yi))，则对于所有i要么有\|yi-xi-a\|<=d要么\|yi+xi-b\|<=d，于是我们可以枚举d，将所有(xi,yi)变为(xi',yi')=(xi+yi,yi-xi)，则\|xi'-b\|<=d,\|yi'-a\|<=d，问题变为用两条分别与纵横轴平行的相同宽度2d的带状区域是否可以覆盖所有点。可以看到宽度具有单调性，于是可以二分解决。首先按x来看一条宽为2d的带来看可以包含多少点，找出没有被包含的点y最大值和最小值，如果它们之差不大于2d，那么说明d符合条件。

[E. Sereja and Squares](http://codeforces.com/contest/314/problem/E)
-----------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/187/1/E.cpp)

去掉背景，用(lowercase,uppercase)表示括号的左和右，除去Sereja不喜欢的x总共有25个括号，现在给定一个序列，都是左括号或问号表示未知，现在对问号赋值问总共有多少种合法的括号序列。

用f(i,j)表示前i个其中有j个开括号，转移方程为

	f(i, j) = f(i-1, j-1)                      s[i] = a lowercase
	f(i, j) = 25f(i-1, j-1) + f(i-1, j+1)      s[i] = '?'

这是一个n^2的算法，题目给定n=10^5，需要做一些优化否则会超时，经过观察发现n^2个状态中其实只用到了n^2/8种，优化到这个地方就可以过了。更厉害的优化可以看看其他的提交。

Summary
-------

A被我搞的太复杂了，半个小时都没搞定，本不打算做这场比赛了，后来还是想着先把A写了，可以第二天早上交一下，写完A后，又看了下其他题目，感觉C可搞，写完过了sample，此时差不多过了70min，想了一下决定赌我的A和C都是对的，就交了，结果两个都是WA，T_T，后面的时间就一直在想C会出什么问题，最后20min发现漏看了非递减的条件，就说怎么会这么快就搞出来。。但是觉得算法是一致的于是就一直debug，但是直到比赛结束都没搞出来。早上又把A和C看了一下，A是漏减了一项导致WA，C仔细重写后也过了，最后都搞出来了也没什么可怨念的了。啊啊啊，弱爆了。
