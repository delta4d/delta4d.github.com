---
layout: post
title: "[Pacman] not enough random bytes available"
date: 2012-05-25 15:21
comments: true
categories: ["pacman", "archlinux"]
---

pacman更新到4.x中出现了认证系统，需要先执行pacman-key --init来生成key，但是这时可能出现not enough random bytes available，后来查了下发现Linux系统生成随机数是根据系统熵来获得的，这是因为系统熵不够的缘故。系统熵值可以通过

```sh
cat /proc/sys/kernel/random/entropy_avail
```

来查看。所谓熵就是系统的混乱程度。

如果你在图形界面的话，那么只要鼠标动几下就可以获得很高的熵值。

如果在字符界面的话，那么可以用手随便敲一些命令，无效的也可以，反正我是对着键盘乱敲的= =，这时可能会使得熵值达到3000多一点，这样执行pacman-key可能还是会报熵值不足，不要着急按Ctrl+C，这时你可以通过Ctrl+Alt+F2切换到tty2，然后继续对着键盘一阵乱敲，然后切换到tty1，就会发现命令已经执行完了。其实只要做一些大量IO的事情就可以了，比如cp一个大体积文件，或者使用dd等等，不一一说明了。

pacman更新好后，pacman.conf可能需要改一下，查看/etc目录会发现有两个pacman配置文件，一个是旧的pacman.conf，一个是4.x的pacman.conf.pacnew，只需要将后者覆盖前者就可以了，记着要对前者备份哦。新的配置文件中有SigLevel选项，默认应该是关闭的，你可以根据需求进行选择：

* Required: 强制检查签名
* Optional: 如果签名存在就检查，但会接受未签名软件包和数据库
* Never:不会进行任何签名检查。

如果开启签名的话，后面安装软件的过程中可能还会遇到一些其他问题，这里就不一一说明了。。
