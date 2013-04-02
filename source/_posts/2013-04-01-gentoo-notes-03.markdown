---
layout: post
title: "gentoo notes 03 - programs"
date: 2013-04-01 21:45
comments: true
categories: ["gentoo"]
---

稍微总结下gentoo下常用的一些软件。

Emerge
------

> emerge是使用[portage](http://en.wikipedia.org/wiki/Portage_(software))的一个工具。

*	查询emerge相关选项的含义
	
		$ man emerge

*	更新portage树

		# 使用rsnyc
		$ emerge --sync
		# rsync不能用时，使用gentoo每天的portage快照
		$ emerge-webrsync

*	查询软件

		# 搜索所有名字里包含pdf的包
		$ emerge --search pdf
		# 在包描述里查询关键字
		$ emerge --searchdesc pdf
		# 或
		$ emerge -S pdf
	
	当然还有更好更快的方法，这个在后面会提到。

*	安装软件
	
		# 安装gnumeric
		$ emerge gunmeric
		# 假装安装软件。在你不确定这个软件都要装哪些包时，可以用这个命令
		$ emerge --pretend gnumeric
		# 只下载源码，可在/usr/portage/distfiles目录下找到
		$ emerge --fetchonly gnumeric

*	查找已安装包文档

	doc USE决定了包文档是否被安装

		# 以alsa-lib为例
		$ emerge -vp alsa-lib
		[ebuild  N    ] media-libs/alsa-lib-1.0.14_rc1  -debug +doc 698 kB

	后面会提到有更好的工具。

*	删除软件

		# 删除gnumeric
		$ emerge -unmerge gnumeric

*	更新系统

		# 更新系统
		$ emerge --update --ask world
		# 更新依赖。否则只会更新列在/var/lib/portage/world里的软件
		$ emerge --update --deep world
		# 上面的命令还是没有更新全部包，因为在装某个软件时，只有在编译时依赖，而在编译完后就不再需要了。
		$ emerge --update --deep --with-bdeps=y world # bdeps = build dependencies
		# 如果USE标记有改变
		$ emerge --update --deep --with-bdeps=y --newuse world

		# 更新整个系统的话，一般执行以下的命令
		$ emerge --sync
		$ emerge -avuND world
		$ revdep-rebuild # from gentoolkit

*	一些标记

	更新系统或装软件时经常会看到一些字母，或者不同的颜色，这些都是有自己特定的意义的

	**State flags**

		B 被已安装的包拦截（阻挡？block怎么翻译比较好= =）
		b 被已安装的包拦截（但是冲突会自动解决）
		I 交互式包安装（需要用户输入）
		N 新的包，未安装
		S 安装到一个新的slot（怎么翻。。）
		D 降级到一个老版本（downgrade）
		U 升级到一个新版本（upgrage）
		R 重新安装相同的版本，很可能时由于新的USE标记或有标记被移除
		F 下载受限，必须手动下载（比如oracle java诶，要点I agree啊。。）
		f 已经下载过啦（nothing to do，yay）
		r 重新安装（很可能是slot或sub-slot的问题）

	**USE flags**
	
		未被高亮的USE是不变或启用
		USE（黄绿蓝）前的减号（-）表示该标记被禁用
		USE后的百分号（%）标志着新加入或被移除，和yellow一样
		USE后的星号（*）表示它的意义/域有所改变，和green一样
		USE两侧的括号()表示它当前被屏蔽（可能由于当前的平台不支持此标记）

	**Color**

		red USE被启用或不变
		yellow USE被添加，移除或屏蔽
		green USE自从上次安装后被改变了，和（*）已一样
		blue USE被禁用，和减号（-）一样

*	安装软件again

	不能成功安装时请仔细查看emerge输出，添加相应的内容到/etc/portage/的package.keywords,package.license,package.mask,package.unmask,package.use等文件。

Gentoolkit
----------

> 这是其他发行版不存在的一个工具，可以说它在一定程度上代表了Gentoo，位于app-portage/gentoolkit。看到kit应该就知道了，这其实是一个工具包，而不是一个单一的软件。接下来一个一个介绍。

*	安装

		emerge gentoolkit

*	equery

	equery用来显示系统上包的信息。可以使用`equery --help`列出相关选项，更详细的可以`man equery`

		# 查找一个文件来自哪个包
		$ equery belongs /usr/bin/xmms
		[ Searching for file(s) /usr/bin/xmms in *... ]
		media-sound/xmms-1.2.10-r9 (/usr/bin/xmms)
		# 使用-f，可以使用正则表达式，使用-e可以在找到一个匹配后立刻终止
		
		# 检查包的完整性
		# 检查md5和时间戳来检查一个可能被冲突，替换或移除的包
		$ equery check gentoolkit
		[ Checking app-portage/gentoolkit-0.2.0 ]
		 * 54 out of 54 files good
		# 配置文件改变可能被认为not good

		# 依赖
		# equery可以用来查询一个包的直接依赖
		$ equery depends pygtk
		[ Searching for packages depending on pygtk... ]
		app-office/dia-0.93
		dev-python/gnome-python-2.0.0-r1
		gnome-extra/gdesklets-core-0.26.2
		media-gfx/gimp-2.0.4
		x11-libs/vte-0.11.11-r1
		# equery可以查询一个特定包的依赖图
		$ equery depgraph cdrtools

		# 查询属于一个ebuild的文件
		$ equery files gentookit

		# 查找使用特定USE标记的包
		$ equery hasuse mozilla

		# 列出包
		$ equery list gentoolkit

		# 计算包大小
		$ equery size openoffice-bin

		# 特定包的USE
		$ equery uses ethereal

		# 查找ebuild位置
		$ equery which cdrtools

*	euse

	euse用来列出，启用或禁用USE标记。

		# 获取当前活动的USE标记
		$ euse -a

		# 启用USE标记
		$ euse -E 3dfx # /etc/make.conf会被改变，备份为/etc/make.conf.euse_backup

		# 禁用USE标记
		$ euse -D 3dfx
	
*	glsa-check

	glsa-check主要用来跟踪GLSA's（Gentoo Linux Security Advisory）。

		# 列出未被应用的GLSAs
		$ glsa-check --list

		# 特定GLSAs的信息
		$ glsa-check --dump 200812-20

		# 测试特定GLSAs的易碎性
		$ glsa-check --test 200812-20

		# 自动应用GLSAs
		$ glsa-check --fix 200901-15

*	revdep-rebuild

	Gentoo的一个反向依赖重建工具。经常在系统更新完后运行此命令。
	
		# pretend mode
		$ revdep-rebuild -p
		# normal mode
		$ revdep-rebuild

*	eread

	显示portage的elog文件。可以在makefile里设置保存的elog文件的相关变量。

		# file location: /etc/make.conf
		PORTAGE_ELOG_GLASSES="log"
		PORTAGE_ELOG_SYSTEM="save"

	一旦设置好后，运行`eread`就可以了。

*	eclean
	
	eclean用来移除陈旧的源码和二进制文件。

Portage-utils
-------------

> Portage-utils提供了一组快速的工具，但是相对gentoolkit在功能上有所限制，这也是由于追求速度所牺牲的。

*	安装

		emerge portage-utils

*	使用

		# qfile找到一个文件属于哪个包
		$ qfile /etc/fonts/fonts.conf

		# qcheck检查包的完整性
		$ qcheck portage-utils

		# qdepends列出包依赖
		$ qdepends -a pygtk

		# qlist列出属于某个ebuild的文件
		$ qlist vim

		# quse查找包的USE标记
		$ quse firefox

		# qsize查询包大小
		$ qsize vim

		# qsearch查找portage树
		$ qsearch terminus
		$ qsearch -H terminus # terminus作者的主页是啥？
		$ qsearch -S "jabber client" # 需要一个jabber clinet？

		# qlop查询emerge log信息
		$ qlop -tH perl # 装perl大概要多长时间
		$ qlop -c # 看看emerge现在在干什么^_^

Eix
---

> 快速查找portage树。我现在基本都用eix来查找。

*	安装

		emerge eix

*	更新缓存
		
		$ eix-update
		$ eix-sync # will run emerge --sync first

*	使用

		$ eix --help
		$ eix emer # 查询名字里有emer的包
		$ eix -C app-portage emer # 在给定类型里查找
		$ eix -I -C app-portage porta # 查询已安装的包
		$ eix -S description # 查找包描述
		$ eix -S -c corba # 查找包描述并且打印完整的列表
		$ eix-remote update # 更新overlay缓存
		$ eix-test-obsolete -c -b # 陈旧和遗失包查询

Layman
------

> 用来管理overlays的工具。

*	安装

		emerge layman

*	配置

		(for layman 1.1)
		$ echo "source /usr/portage/local/layman/make.conf" >> /etc/make.conf

		(for layman 1.2)
		$ echo "source /usr/local/portage/layman/make.conf" >> /etc/make.conf

		(for layman 1.3 and later)
		$ echo "source /var/lib/layman/make.conf" >> /etc/make.conf

*	使用

		# 列出可用overlays
		$ layman -L

		# 安装一个overlay
		$ layman -a <overlay-name>

		# 更新overlay
		$ layman -S

References
----------

* [A Portage Introduction](http://www.gentoo.org/doc/en/handbook/handbook-x86.xml?part=2&chap=1)
* [Gentoo Portage Wiki](http://en.wikipedia.org/wiki/Portage_(software))	
* [Portage Wikipedia](http://en.wikipedia.org/wiki/Portage_(software))
* [Gentoolkit](http://www.gentoo.org/doc/en/gentoolkit.xml)
* [Portage-Utils](http://www.gentoo.org/doc/en/portage-utils.xml)
* [Eix Homepage](http://eix.berlios.de/)
* [Gentoo Overlays](http://www.gentoo.org/proj/en/overlays/userguide.xml)
