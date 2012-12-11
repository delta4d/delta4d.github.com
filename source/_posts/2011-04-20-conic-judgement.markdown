---
layout: post
title: "二次曲线的判断"
date: 2011-04-20 19:31
comments: true
categories: ["mathematics", "conic"]
---

之所以想起这个问题是因为[ZOJ3488](http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemCode=3488)，题目是说给定二次曲线方程ax^2+bxy+c^2+dx+ey+f=0，判断曲线类型，其中保证曲线肯定存在且b=0，我准备更一般的来研究一下这个问题

1. 若b=0，则：

1.1 a或c中有一个为0，则位抛物线

1.2 方程可以化为：a(x+d/(2a))^2+c(y+e/(2c))^2=d^2/(4a)+e^2/(4c)-f

记D=d^2/(4a)+e^2/(4c)-f

若D=0，则该方程表示点(-d/(2a), -e/(2c))

若ac>0,a=c,acD>0,则表示圆

若ac>0,a!=c,acD>0,则表示椭圆

若ac<0,表示双曲线

其他情况方程不表示任何曲线，方程有误



2. 若b!=0，则：

ax^2+bxy+cy^2+dx+ey+f=0 -----------（1）

做旋转变换z‘=z*e^(i*t)

那么x=x*cos(t)-y*sin(t), y=x*sin(t)+y*cos(t)

带入（1）则xy项的系数为：

-2a*cos(t)sin(t)+b(cos(t)*cos(t)-sin(t)*sin(t))+2c*sin(t)cos(t)

=-a*sin(2t)+b*cos(2t)+c*sin(2t)

=(c-a)sin(2t)+b*cos(2t)

令它为0，则tan(2t)=b/(a-c)，当a=c时 tan(2t)=infinity，2t=pi/2

因为tan(2t)=b/(a-c)

那么cos(2t)=(a-c)/sqrt(b^2+(a-c)^2)，sin(2t)=b/sqrt(b^2+(a-c)^2)

(a*cos^2(t)+b*sin(t)cos(t)+c*sin^2(t))x^2+(a*sin^2(t)-b*sin(t)cos(t)+c*cos^2(t))y^2+(d*cos(t)+c*sin(t))x+(-d*sin(t)+c*cos(t))y+e=0

(a*(1+cos(2t)/2)+b*sin(2t)/2+c*(1-cos(2t))/2)x^2+(a*(1-cos(2t)/2)-b*sin(2t)/2+c*(1+cos(2t))/2)y^2+(c*sin(t))x+(-d*sin(t)+c*cos(t))y+e=0

(a+c+sqrt(b^2+(a-c)^2))/2*x^2+(a+c-sqrt(b^2+(a-c)^2))/2*y^2+c*sin(t))x+(-d*sin(t)+c*cos(t))y+e=0

转化为情况1
