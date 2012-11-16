---
layout: post
title: "VIM function already exists"
date: 2012-11-16 23:55
comments: true
categories: VIM
---

由于每次修改vimrc后都要重启vim才会在读取配置文件，感觉非常麻烦。（汗，认识vim时间也不短了，但一直木有进步。。）后来觉得可以source，于是自定义了快捷键

	map <silent> <leader>ss :!source ~/.vimrc

但是显示了一大堆错误，大家可能已经看出来了，对的，不应该加*!*的，vimrc不应该由shell来解释，而应该由vim解释，于是修改为：

	map <silent> <leader>ss :source ~/.vimrc

但是这样会出现*function xxx already exists add ! to replace it*的警告，按照提示在function后加上!就可以消除警告了，这是由于如果函数出现过，加入!会redifine。555，rbl。
