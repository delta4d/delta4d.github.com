---
layout: post
title: "git项目嵌套"
date: 2013-02-16 19:38
comments: true
categories: ["git"]
---

以前看到有的github项目有如下的情况：

![](/images/upload/2013-02-16-git-submodule.png)

就是一个git项目的子目录也是一个项目，在github上看起来就像链接一样。今天终于发现，这其实是git的submodule，具体可参考[这里](http://git-scm.com/book/en/Git-Tools-Submodules)，也可以`git submodule --help`

具体做的时候可以这样：

```
cd repo_dir
git submodule git@github.com:yourname/yourrepo.git
```
