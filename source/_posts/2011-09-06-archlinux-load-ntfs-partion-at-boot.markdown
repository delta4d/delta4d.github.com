---
layout: post
title: "Archlinux开机加载NTFS分区"
date: 2011-09-06 09:47
comments: true
categories: ["linux", "fstab"]
---

修改/etc/fstab文件，添加如下内容：

```
/dev/sda1 /media/sda1 ntfs-3g defaults,locale=zh_CN.UTF8 0 0
/dev/sda2 /media/sda2 ntfs-3g defaults,locale=zh_CN.UTF8 0 0
```

需要注意几点：

1. /dev/sdax为电脑上对应的分区，视具体情况而定
2. /media/sdax为/media中的目录需要提前建立好，名字任意
3. ntfs-3g需要提前安装好，其后所接的参数可以通过手册来查询

reference:

* [fstab具体参数意义及书写格式](http://en.wikipedia.org/wiki/Fstab)
* man fstab
