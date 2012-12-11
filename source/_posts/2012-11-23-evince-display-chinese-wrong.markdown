---
layout: post
title: "Evince中文显示错误"
date: 2012-11-23 19:06
comments: true
categories: ["linux","evince"]
---

今天用Evince打开一个pdf，本来应该显示中文的地方都是空白，google后发现是[poppler](http://poppler.freedesktop.org)的问题，缺少了编码集poppler-data，那么安装好这个包就可一了

on debian

```
sudo apt-get install poppler-data
```

on archlinux

```
sudo pacman -S poppler-data
```

于是基于poppler的pdf阅读器就都有可能出现这个问题，详见[PDF Readers Using Poppler](http://en.wikipedia.org/wiki/Poppler_(software)#PDF_readers_using_Poppler)
