---
layout: post
title: "gentoo notes 01"
date: 2013-01-27 19:26
comments: true
categories: ["gentoo"]
---

Why Gentoo
----------
久仰Gentoo大名，过去一年断断续续在虚拟机上装过好几次Gentoo了，一直想在实体机上试试，于是趁放假的时候折腾折腾。

Start
-----

Gentoo是编译安装的，于是可以进行相关编译选项优化。同时Gentoo是可以滚动升级的，这样方便不少。开始装机的时候还是想着自己编译内核的，结果开机后不断panic，把我也搞的panic了，于是老老实实用genkernel，以后有机会在好好编译内核吧，先把系统起来在说。很多用户觉得Gentoo编译太费时间了，所以不选择Gentoo，但装机大多都是可以一劳永逸的，配置好后并不会频繁的装软件了。装好基本系统其实还是比较快的，使用genkernel也只用了一个小时左右，装好X然后装DE便会比较慢了，键盘控，喜欢轻量级的话装个WM是便会很快了。

Errors
------

```
!!The filesystem mounted at /dev/sdb3 does not appear to be a valid /. Try again 
!!Could not find the root block device in. 
Please specify another value or: press Enter for the same , type "shell" ......
```

可能是grub.conf写错了，需要注意一下两个变量：

* `root (hd0,0)`指/boot在/dev/sda1。(hd0,0-3)表示四个主分区，(hd0,>3)表示逻辑分区，其中grub从0开始，而linux从1开始表示分区。
* `real_root=/dev/sdax`指/在/dev/sdax


startx的时候出现no screens found
   可能是显卡驱动装的不对，可以执行`emerge -av $(qlist -IC x11-drivers)`，我的是nvidia，在make.conf中设置好`VIDEO_CARDS="nvidia"`，然后装好n卡驱动，在执行`nvidia-xconfig`后就可以了。

emerge complains  [click me!](http://www.gentoo.org/doc/en/handbook/handbook-x86.xml?part=2&chap=1)

Why Gentoo Again
----------------
1. 漂亮的字符界面。第一次虚拟机装Gentoo时的最深的印象。
2. 强大的portage和emerge。portage告诉emerge在哪里下载源代码，编译选项是什么。
3. overlay？就目前的感觉想archlinux的aur。里面应该有不少好玩的东西。
4. 详尽的文档。
5. ......

实体机用了才不到两天，还有好多好多东西需要学习和探索。Happy Hunting。

DE vs WM
--------
一开始想装wm的，因为在[archlinux forum](https://bbs.archlinux.org/viewforum.php?id=47)里看到很多wm非常漂亮，还有很多[轻量级软件](https://bbs.archlinux.org/viewtopic.php?id=138281)提供选择。也确实一开始试的是openbox和monsterwm的，还是不习惯。最后换成了KDE，其实KDE以前没怎么用过（其实连linux都没怎么用过，总是不知不觉切会win后就不再进入linux了），就目前来看KDE表现还是不错的，但还是有些问题没有解决。。一开始挺好的，但是感觉有些不稳定，无法启动kde程序，gtk倒是没问题，后来删除了~/.kde4恢复默认设置倒是好了。另外qtconfig配置无法保存，这个直接导致了有些qt程序字体比较难看等。

Octopress in Gentoo
-------------------

```
...custom_require.rb:36:in 'require': cannot load such file auto_gem (LoadError)
...custom_require.rb:36:in 'require'
```

`unset RUBYOPT` fix it.

```
Building site: source -> public
File "<string>", line 1
import sys; print sys.executable
^
SyntaxError: invalid syntax
```

Follow [this](http://blog.gonzih.org/blog/2011/09/21/fix-octopress-pygments-error-on-arch-linux/). But I change rubypython from 0.5.3 to 0.6.3 in Gemfile.lock, it fixs everything I ran into..

Reference
---------
1. [Google](http://www.google.com)
2. [Gentoo Handbook](http://www.gentoo.org/doc/en/handbook/)
3. [Gentoo Xorg](http://www.gentoo.org/doc/en/xorg-config.xml)


What's Going On
---------------
Keep Using, Keep Hunting。有没有gentoo notes 02我也不知道。。
