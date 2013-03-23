---
layout: post
title: "goagent on linux"
date: 2013-02-03 17:11
comments: true
categories: ["linux"]
---

不知从什么时候开始，每次装机后翻墙变为第一件要做的事。[goagent](https://code.google.com/p/goagent/)已经进入了2.x时代，配置也方便了不少，其实跟着wiki走一遍就好了。

Install
-------
1. 在[GAE](https://developers.google.com/appengine/)建立自己的工程，记住工程的名字
2. 将local/proxy.ini中的appid改为你自己的刚刚建立的工程名
3. 进入server目录执行`uploaddir=python python uploader.zip`将文件上传到服务器
4. 进入local执行`python2 proxy.py`
5. 安装插件SwitchySharp，可以在[chromewebstore](https://chrome.google.com/webstore/category/home)找到

Issues
------
1.	gevent or pyopenssl disabled

		sudo emerge -va gevent
		sudo emerge -va pyopenssl

	add keywords changes according to output msg
2.	twitter | youtube 无法登录
	打开chrome导入local目录下的CA.crt即可

Start at Boot
-------------
local目录下有自启动脚本，适用于win，gentoo下可以这么做

```coffeescript
#!/usr/bin/env sh
#/etc/local.d/goagent.start

nohup python2 path/to/proxy.py &
```

Reference
---------
[Gentoo run a command on boot](http://en.gentoo-wiki.com/wiki/Run_a_command_on_boot)
