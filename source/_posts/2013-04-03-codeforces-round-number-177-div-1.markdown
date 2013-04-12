---
layout: post
title: "Codeforces Round #177 (Div. 1)"
date: 2013-04-03 13:11
comments: true
categories: ["codeforces"]
---

[A. Polo the Penguin and Strings](http://www.codeforces.com/contest/288/problem/A)
---------------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/177/1/A.cpp)

有这样一种字符串，它只含有k个不同的字母，长度为n，并且相邻字符不相同。给定n和k，输出字典序最小的那个，不存在的话输出-1。

首先考虑不存在的情况，即不能满足相邻字符不相同，或不够k个字符，即n\>&&1k=1和k\>n的情况。存在的情况下，由于要求字典序最小所以贪心使用ab的重叠就好了。

[B. Polo the Penguin and Houses](http://www.codeforces.com/contest/288/problem/B)
--------------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/177/1/B.cpp)

有n个房子编号1-n，每个房子有一个门牌号(1-n)，房子x的门牌号记为Px，注意门牌号是可以重复的，当你到房子x时，下一步到Px，然后到P(Px)...，现在已知：

	1. 从1-k出发，都会回到1
	2. 从k+1-n出发，都不会回到1
	3. 从1出发，经过非0步回到1

现在给定n，k，确定有多少种门牌号编排方案。其中k\<=8。

由条件知，从1-k出发都不可能到达k+1-n，从k+1-n出发也不可能到1-k。于是由于k的范围很小，可以暴力出1-k的方案数。k+1-n为(n-k)^(n-k)种，然后乘起来就好了。

[C. Polo the Penguin and XOR operation](http://www.codeforces.com/contest/288/problem/C)
---------------------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/177/1/C.cpp)

0-n这n+1个数有一个排列{pi}，对于每一个排列对应一个数值p=(0^p0)+(1^p1)+...+(n^pn)，求p的最大值。

采用贪心。对于每个数x，去掉x的二进制最高位后，然后取反得到x'，则x'\<x，使得x^x'最大。即对于任何一个x都存在一个x'\<x与之对应，并且这种对应是唯一的，即不会有x!=y，而x'=y'，于是从n开始倒序搜索，总可以进行这样的匹配。

[D. Polo the Penguin and Trees](http://www.codeforces.com/contest/288/problem/D)
-------------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/177/1/D.cpp)

给一棵树，查询这样元组(a,b,c,d)的个数，其中a\<b,c\<d，并且a和b的最短路与c和d的最短路没有公共节点。

从反面考虑，不考虑条件，所有元组的个数为C(n,2)^2，然后去除最短路有公共点的元组。若公共点为x，则最短路经过x的点对有两种：

	1. a和b分别在x的两个儿子形成的子树中
	2. a在x为根的子树中，b在除x子树的剩余节点中

a和b都在除x子树的剩余节点是不行的，这样最短路肯定不会通过x了。这样的话只需要dfs一遍就可以了。

[E. Polo the Penguin and Lucky Numbers](http://www.codeforces.com/contest/288/problem/E)
---------------------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/177/1/E.cpp)

lucky number是十进制位只含有4和7的数，现在给定一个区间[l,r]，其中l和r都是lucky number，位于[l,r]之内的lucky number为{ai}，求a1a2+a2a3+...+a(n-1)an。

采用dp解决，从高位向低位扫描。只需要计算[44..4,l]和[44..4,r]的结果就可了，然后二者相减就得到了结果。比如[444,747]，观察如下图表，每一次的{ai}，对应到下一次为{10ai+4,10ai+7}，只有最后一个an有所区别，有可能没有10an+7。

	1-----2------3

	       /-- 444
	   - 44 -- 447
	  /    /-- 474
	4 -- 47 -- 477
	       /-- 744
	7 -- 74 -- 747

看到这样的关系后就可以写转移方程了，设F[n]为题目所求，K[n]为当前数{ai}的多少，比如上图中K[1]=1,K[2]=3,K[3]=6。

$$

\begin{align*}

& F_{n+1} = \sum_{i=1}^{K_n-1}(10a_i+4)(10a_i+7) + \sum_{i=1}^{K_n-1}(10a_i+7)(10a_{i+1}+4) \\

& F_{n+1} = 100F_n+100\sum_{i=1}^{K_n-1}a_i^2 + 220\sum_{i=1}^{K_n-1}a_i+56(K_n-1)+70(a_{K_n}-a_1) \\

\end{align*}

$$

上面是对于当前处理字符为4的情况，7的情况只需要再加上最后一项$$(10a_{K_n}+4)(10a_{K_n}+7)$$即可，然后可以设

$$
	S_n = \sum_{i=1}^{K_n-1}a_i, \quad T_n = \sum_{i=1}^{K_n-1}a_i^2
$$

求出对应的递推关系，就可以了。

Summary
-------

A，B，C较以往的比赛难度并不算太高，算是比较幸运的在一个小时内1y了前三题，后面一个小时就打酱油了，还是缺乏砍难题的能力啊，啊啊啊，弱爆了弱爆了。

D比赛的时候走偏了，我首先去计算没有公共节点的元组，然后再在这些元组中去去重，始终想不到好的方法，加上自己也确实缺少能够搞定D的信心，后面也就没有再深入去想了。如果开始从更大的角度从反面考虑的话，或许会有一些想法。啊啊，以后做题一定要霸气啊，管他ABCDE，嗯嗯。还有就是要从更多角度去思考问题，但是很多时候真的找不到合适的角度诶，要多做题，多看大牛代码，多看大牛文章。

E花了一天终于搞定了，略兴奋。主要是要发现前后ai之间的关系，然后就只剩下一些计算了。
