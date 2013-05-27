---
layout: post
title: "An Useful Congruence Identity"
date: 2013-05-27 23:05
comments: true
categories: ["number theory", "mathematics"]
---

有这样一个同余恒等式：

$$
\forall a, m, n \geq \varphi(m)\ \ a^n \equiv a^{n\ mod\ \varphi(m)+\varphi(m)}\ (mod\ m)
$$

要证明此式只需证明$$a^n \equiv a^{n-\varphi(m)}\ (mod\ m)$$，即$$m\vert a^{n-\varphi(m)}(a^{\varphi(m)}-1)$$

设m质因子分解为$$\prod_{i=1}^{t}p_{i}^{\alpha_i}$$，那么我们只需证明对任意i有$$p_i^{\alpha_i}\vert a^{n-\varphi(m)}(a^{\varphi(m)}-1)$$，对于每个i分两种情况讨论：

(1) (a, pi)=1

根据[欧拉定理](http://en.wikipedia.org/wiki/Euler's_theorem)，$$a^{\varphi(p_i^{\alpha_i})}\equiv 1\ (mod\ p_i^{\alpha_i})$$

因为$$\varphi(p_i^{\alpha_i})\vert \varphi(m)$$，所以有$$a^{\varphi(m)}-1\equiv 0\ (mod\ p_i^{\alpha_i})$$

(2) (a, pi)!=1，因为pi是素数则(a, pi)=pi

此时$$(a^{\varphi(m)}-1, p_i^{\alpha_i})=1$$，我们只需证明：$$ p_i^{\alpha_i}\vert a^{n-\varphi(m)} $$

即$$n-\varphi(m)\geq \alpha_i$$

假设$$n-\varphi(m)\geq \varphi(m)$$，则

$$
\varphi(m)=\prod_{i=1}^{t}p_i^{\alpha_i-1}(p-1)\geq p_i^{\alpha_i-1}\geq 2^{\alpha_i-1}=(1+1)^{\alpha_i-1} =\sum_{i=0}^{\alpha_i-1}\binom{n}{i} \geq \sum_{i=0}^{\alpha_i-1}1 = \alpha_i
$$

所以有$$p_i^{\alpha_i}\vert a^{n-\varphi(m)}$$

综上有：当$$n\geq 2\varphi(m)$$时$$p_i^{\alpha_i} \vert a^{n-\varphi(m)}(a^{\varphi(m)}-1)$$

即$$a^n \equiv a^{n-\varphi(m)}\ (mod\ p_i^{\alpha_i})$$

当取遍所有i则

$$
a^n \equiv a^{n-\varphi(m)}\ (mod\ m=\prod_{i=1}^{t}p_i^{\alpha_i})
$$

于是可以不断的减去$$\varphi(m)$$，直到$$2\varphi(m)>n\geq \varphi(m)$$，这也就是最后为什么会加上$$\varphi(m)$$的原因。

下面有一些相关的题目：

* [ZOJ 2674 Strange Limit](http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemCode=2674)
* [CodeForces Round 17 D Notepad](http://codeforces.com/contest/17/problem/D)
