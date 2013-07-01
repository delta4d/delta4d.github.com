---
layout: post
title: "Codeforces Round #190 (Div. 1)"
date: 2013-06-29 22:02
comments: true
categories: ["codeforces"]
---

[A. Ciel and Robot](http://codeforces.com/contest/321/problem/A)
-------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/190/1/A.cpp)

有一个bot位于xoy平面原点，给定一个移动序列s包含'UDLR'，分别表示向上，下，左和右各移动一个单位，该指令序列s可以重复执行任意此，现在给定坐标(a,b)问是否可以经过有限次指令后到达。

设置行到s(i)后bot坐标为(xi,yi)，执行完s后的坐标为(sx,sy)，则若能够到达(a,b)则存在非负整数k和i使得a=xi+ksx,b=yi+ksy。枚举i就可以了，算法看起来虽然比较简单，但实际写起来还是有很多需要注意的地方。

[B. Ciel and Duel](http://codeforces.com/contest/321/problem/B)
------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/190/1/B.cpp)

Ciel和Jiro玩这样一个牌类游戏，Jiro有n张牌，每张有两个属性：position(Attack或Defense)，strength，Ciel有m张牌，现在轮到Ciel进攻，他可以重复置行以下操作：

	1. 选择自己的一张牌X，这张牌之前没有用过
	2. 如果Jiro没有手牌，那么他被伤害X
	3. 否则由Ciel选择Jiro的一张手牌Y：
	   * Y的position为Attack，那么必须有(X's strength) >= (Y's strength)，Jiro获得伤害(X's strength)-(Y's strength)，手牌Y不可以再用
	   * Y的position为Defense，那么必须有(X's strength) > (Y's strength)，Jiro没有伤害，手牌Y不可以再用

现在求Jiro能够受到的最大伤害是多少

如果Ciel没有将Jiro的手牌拿完，那么可以看到选择Def牌是不会增加伤害的，于是只需要考虑Atk，于是采用贪心，枚举Ciel可以将自己最大的多少张牌打完。

如果Ciel将Jiro的手牌取完，要使结果最大，那么用刚好大于Def的牌去对付Def就可以了，然后看是否可以取完Atk牌。更多的解法，可以官方[editorial](http://codeforces.com/blog/entry/8192)

[C. Ciel the Commander](http://codeforces.com/contest/321/problem/C)
-----------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/190/1/C.cpp)

有一棵树，给每个点赋一个等级(A-Z)，若两个点等级相同，那么连接这两个点的路径里至少有一个点的等级大于两个端点。问是否存在这样的赋值方案。

假若最大规模，有一个10^5的链，那么可以看到先对中点x赋值A，那么左右分为两棵子树，如果这两个树中有相同的等级，那么必须通过x，于是只需让两棵子树的最大等级为B就可以了，然后依次找子树的中点，递归的做下去，可知log2(10^5)<26，于是方案是存在的。

现在需要拓展线段的中点到树的中点（重心），重心即使得他的最大子树的大小最小，具体可以看[sgu134](http://acm.sgu.ru/problem.php?contest=0&problem=134)和Google。

[D. Ciel and Flipboard](http://codeforces.com/contest/321/problem/D)
-----------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/190/1/D.cpp)

有一个nxn的方阵，n为不大于33的奇数，有这样一个操作，取m=(n+1)/2，在nxn的方阵中取一个mxm的子方阵，其中的数全部取其相反数，现在问通过这种操作方阵中所有数的和的最大值是多少？

共有mxm个子方阵，即共有m^2种操作，由于重复同一种操作是没有效果的，于是每种操作只会做0次或1次，共有2^(m^2)种结果，不同的操作序列产生不同的结果。

假若一个方格最后改变了符号那么赋值1，否则0，即这个赋值方阵为s，那么操作中蕴含有这样的等式：

	s[i][j] ^ s[i][m] ^ s[i][j+m] = 0    1 <= j < m
	s[i][j] ^ s[m][j] ^ s[i+m][j] = 0    1 <= i < m

这是因为(i,j)(i,m)(i,j+m)同一操作中要么有0个要么有2个被选中，于是只需要左上角mxm的方阵就可以决定nxn的方阵。并且给出左上角方阵的一个布局那么肯定有一个操作序列与其对应，这是因为左上角mxm的方阵恰好有2^(m^2)种布局，于是刚好和操作序列产生的布局一一对应。

于是现在可以枚举第m列的情况，然后决定第m行的情况，接下来贪心就可以了。

[E. Ciel and Gondolas](http://codeforces.com/contest/321/problem/E)
----------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/190/1/E.cpp)

有n个人，现在要对这n个人分组，每组的人编号都是连续的，每两个之间有一个不熟悉的值，一个组的不熟悉值为组内两两不熟悉值的总和，现在求所有分组中不熟悉值最小是多少，其中n<=4000，最大的分组数k为800。

dp，设f(i,j)表示前i个人分为j组的最小不熟悉值总和，则

	f(i, j) = min(f(k, j-1) + unfamilliar(k+1..i))
	unfamilliar(a, b) = u(a, a+1) + u(a, a+2) + ... + u(a, b) + u(a+1, a+2) + ... + u(b-1, b)

给定u则unfamilliar同样采用dp可以在n^2时间内求得，f转移方程复杂度为n，f状态共有nk，则复杂度共kn^2，需要对dp进行优化。

优化的地方缩小k的范围，设t(i,j)=min{k\|f(i,j)=f(k,j-1)+unfamilliar(k+1..i)}，则有t(i,j-1)<=t(i,j)<=t(i+1,j)。

Summary
-------

A写的比较仔细，1y。然后就一直再搞B，感觉匹配棵搞，网络流也可搞，但就是搞不出来，感觉可以贪心，但又没有一个仔细的贪心策略，感觉脑子轻飘飘的，不会思考一样，最后虽然过了pretest，但挂了finaltest，中间有想过C，其实算法自己想的是对的，但是复杂度估计错了，就没有再搞，这个已经不是第一次了，现在感觉复杂度的估算是一个比较严重的问题了，会影响对于算法的思考，严重不扎实啊。啊啊啊，弱爆了弱爆了。
