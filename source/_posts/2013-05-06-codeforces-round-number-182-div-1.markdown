---
layout: post
title: "Codeforces Round #182 (Div. 1)"
date: 2013-05-06 13:19
comments: true
categories: ["codeforces"]
---

[A. Yaroslav and Sequence](http://www.codeforces.com/contest/301/problem/A)
--------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/182/1/A.cpp)

给定一个2n-1长度的数列，可以选取这个数列中的n个数然后取其相反数，这样的操作不限次数。问这个数列所有元素和最大可能是多少。

比如头两次选取重复的部分的长度为L，那么两次操作后改变符号的元素为2(n-L)个，若能使2(n-L)=n+1，那么接下来在做一次操作，便可以达到只改变一个元素的效果，此时L=(n-1)/2，只有当n为奇数的时候才有可能，当n为偶数时能够改变的最小元素个数为2，因为假若经过若干次变换达到了只改变一个元素的效果，考虑不变量，假设每操作一个元素那么加1，则操作k次后的和为kn。那么考虑只有一个元素改变的情况，其他元素被操作的次数为偶数，于是总的改变元素的次数为奇数，而kn为偶数，矛盾，所以若干次操作最少也会改变两个元素，且这个界是可以达到的，取L=n/2+1就可以了。


[B. Yaroslav and Time](http://www.codeforces.com/contest/301/problem/B)
----------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/182/1/B.cpp)

从i到j需要消耗(\|xi-xj\|+\|yi-yj\|)\*d单位的资源，i可以增加ai单位的资源，但是第二次到i的时候不会重复增加，期间可以购买资源，问从要从1到n最少需要购买多少资源。

由于经过i后不管去向哪里都会增加ai，于是将dis(i,j)可以看作ij曼哈顿距离乘以d减去ai，然后正常求最短路就可以了。


[C. Yaroslav and Algorithm](http://www.codeforces.com/contest/301/problem/C)
---------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/182/1/C.rb)

有这样一个算法A：

	1. A接受一个字符串a为输入
	2. A包含了一些命令。命令i要么为si>>wi，或者si<>wi，si和wi是长度不大于7的字符串，包含数字和'?'
	3. 每一次迭代过程中，A找到最小的一个索引i，使得si是a中的一个子串。若没有找到则算法结束
	4. 用k表示每次迭代找到的命令，将a中si用wi代替，若为>>则继续迭代，若为<>则算法结束
	5. 算法结束后的a作为A的输出

现在给定n（\<=100）个数字，要求每个数字经过算法A后都加一。要求输出一系列的命令构成算法A完成要求，其中对A有如下限制：
	
	1. 每一个命令都是合法的
	2. 命令总数不能超过50
	3. 算法会将每个输入的数字加一
	4. 对于每个数字迭代次数不能超过200

很直接的想法就是对于每个输入x\<\>x+1，然而由于输入有100个，而命令不能超过50，所以这样是不可以的，于是需要想一个适用于所有数字的命令而不是针对特定数字的。

需要用'?'来完成要求，以123来举例，我们首先可以随便找一个数，比如找到了1，将1变为1?，则123-\>1?23，然后不断的将?后移变为123?，然后将3?变为4就可以了，比较麻烦的是需要进位的情况，这样就需要往前处理，比如99变为99?后我们可以将9?变为9??，此时99-\>99??，然后99??-\>9??0-\>??00-\>100。

[D. Yaroslav and Divisors](http://www.codeforces.com/contest/301/problem/D)
--------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/182/1/D.cpp)

给定一个大小为n（\<=200000）的数组p，p为1-n的一个排列，然后有m（\<=200000）个查询l，r，问在p[l..r]中满足pi整数pj的数对ij有多少。

对于每个i在1-n中i的倍数为n/i个，则共有n/1+n/2+...+n/n=nln(n)个满足整除关系的点对，按数组索引预处理出这样的点对，比如对索引i，找出所有的j<=i使得p[j]整除p[i]或p[i]整除p[j]。然后从p左侧开始扫描，引入计数数组c，当扫描到i时，对于区间[1,j]中的每个k，c[k]+=1，则对于查询[l,i]的结果即c[l]。

[E. Yaroslav and Arrangements](http://www.codeforces.com/contest/301/problem/E)
------------------------------

[source code](https://github.com/delta4d/AlgoSolution/blob/master/codeforces/182/1/E.cpp)

定义good array {an}为：

	1. a1 = min{an}
	2. |a1 - a2| = 1, |a2 - a3| = 1, ..., |a(n-1) - an| = 1, |an - a1| = 1

定义great array {br}为：

	1. b1 <= b2 <= ... <= br
	2. 1 <= r <= n, 1 <= bi <= m
	3. {br}的所有排列中有t个good array，且 1 <= t <= k

给定n，m，k求{br}的个数

考虑下goodarray有什么性质。首先若{an}good，则{an-a1}good，所以我们可以固定最小元为0。设x=min{an},y=max{an}，那么y-x\<=n/2（利用这个可以降低这一维的长度），这是因为假若y太大，就不能构成环了，基于同样的原因，[x,y]之间的每个数在{an}中都必须出现，否则会出现断层。其次n为偶数，这是由于a1-an=(a1-a2)+(a2-a3)+(a(n-1)-an)，即a1-an为n-1个1或-1的和，即a1-an于n-1奇偶性相同，而a1-an为奇数，所以n为偶数，这样dp的时候这个维的长度可以减半，这个在具体推导转移方程的时候也可以看出来。最后考虑y的周围两个数，因为y最大，所以y的两侧只能是y-1，这样可以发现如下规律

	0 1 2 1 0 1 0 1 2 1 => 0 (1 2 1) 0 1 0 (1 2 1) => 0 1 0 1 0 1

即可以把(y-1 y y-1)看作y-1，则会构成一个最小值为x最大值为y-1且规模更小的good array，这样就可以适用动态规划了。我们逐个添加元素，需要记录当前处理的元素，当前array大小，当前元素选了多少个，所有排列中有多少great array。如果当前元素i有y个，可见至少需要i-1 x+y个，共有C(x+y-1,x)种选法，然后枚举当前元素选多少个更新就可以了。

Summary
-------

这场由于技术原因最后是unrated，momo作者。正准备写B的时候A被hack了，检查了半天最后发现数组开小了，改完之后就直接睡了，以后要多注意啊。C是一道很好玩的题目，E的dp也很好，最后还是要momo作者。
