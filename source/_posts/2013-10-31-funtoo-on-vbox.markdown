---
layout: post
title: "funtoo on vbox"
date: 2013-10-31 15:00
comments: true
categories: ["funtoo", "gentoo", "vbox", "octopress"]
---

长时间没什么更新，主要是没有再做算法比赛了，不知道自己什么时候能开窍啊，会有那么一天么。。也并未研究些什么，并没有什么需要特别记录的。另外机子上Linux系统也已经不在了，换了机器后想更新下不是特别容易。。

对我来说不管单独使用win还是linux都会有些问题。win下虚拟机装linux，然后putty连接vbox是目前对我来说使用win和Linux的最好方式了。。

至于为什么装[funtoo](http://www.funtoo.org/)，是因为一点点的好奇，还有gentoo已经试过了，再次装的话想图个新鲜。安装步骤并不复杂，跟着官方wiki走一遍就好了。一开始内核用的是debian-sources，因为可以用binary标记，再者也是官方推荐的，但是编译内核的时候一直在compressing cpio那里卡了5个夜还是没结果，后来换成vanilla-sources后，看完一集HIMYM就好了，安装速度出奇的快。需要说明的是，如果使用genkernel的话，可能在安装guile的时候会出现@print无法使用等问题，只要将sys-apps/texinfo降级到4.13-r2就可以了。

装好funtoo后，然后需要putty连接vbox。需要对虚拟机添加一个adapter，选择host-only，然后虚拟机开启ssh，查看eth1 ip地址，putty就可以连接了。为什么要这么麻烦使用putty呢，一可以通过putty提高用户体验，二是如果在虚拟机中搭建网络比如有rails工程的话，那么在win主机中可以通过浏览器访问，这个是我认为最方便的。访问的话只要敲你putty连接的那个ip就可以了，记不住的话可以在win的hosts文件里添加一个别名就好了。如果需要putty正确显示中文的话需要将window-translation-received data assumed to be in which character set设置为utf8。另外还有一个比较重要的是设置共享文件夹，这个首先需要在虚拟机设置中添加共享目录，然后需要安装vbox增强插件，funtoo里`emerge virtualbox-guest-additions`就装好了，记着要开启对应的服务，通过`/etc/init.d/virtualbox-guest-additions start`就可以了，如果需要开机启动服务的话，`rc-update add virtualbox-guest-additions default`就好了，然后敲`rc`就可以重新启动一遍需要的服务。最后`mount -t vboxsf your_share_folder_name your_mount_point`就可以访问共享文件夹了。

装好虚拟机后，开始配置octopress，每次在新环境下配octopress都会有不同的问题。。首先通过以下命令更新一下octopress

	$ cd your_octopress_directory
	$ git remote add octopress https://github.com/imathis/octopress.git
	$ git pull octopress master
	$ bundle install

funtoo/gentoo下的ruby版本管理还是比较方便的，`eselect ruby list`查看可用版本，`eselect ruby set your_favourite_version`就可以了，目前倒是没有特别需要rvm或rbenv。要使用eselect ruby需要安装`eselect-ruby`，默认应该是没有的。funtoo/gentoo下的eselect确实很方便。

如果octopress下出现类似'pygments_code.rb:27:in 'rescue in pygments': Pygments can't parse unknown language'之类的问题，这个是由于所使用python版本引起的，可以通过`eselect python set xxx`来解决。装好机后python默认是3.x，但是现在很多程序都是2.7的。还有一种方法是改掉gems/.../pygments/mentos.py的shebang，将python改为python2就可以了。

最后表示持续关注[ghost](https://github.com/TryGhost/Ghost)的进展，就目前来看应该是不错的，希望以后可以将博客迁移到ghost上。
