---
layout: post
title: "linux终端在同一行输出结果"
date: 2012-12-11 16:00
comments: true
categories: ["linux", "shell"]
---

最近了解了一下[dwm](http://dwm.suckless.org/)，想定制下状态栏，于是首先想写一个检测内存的脚本。脚本并不复杂，但仍然用了好多时间来查找资料，使用linux果然还是要多写程序脚本啊。

{% codeblock mem_used.sh lang:bash %}
#!/bin/bash

function get_mem_used {
	eval `free -m | awk 'NR==3 {printf "used=%.2lf per=%.2lf", $3, 100*$3/($3+$4)}'`
	echo "MEM:$per%($used MB)"
}

while true; do
	echo $(get_mem_used)
	sleep 1
done
{% endcodeblock %}

将会产生如下的输出：

```
MEM:25.07%(758.00 MB)
MEM:25.04%(757.00 MB)
MEM:25.07%(758.00 MB)
```

然后我就想如何能让每条记录都在同一行更新，man echo will be helpful。首先使用-e选项开启转义符，\b即为退格

于是可以这样

```
echo -e "foo"
echo -e "\b\b\bbar"
```

但是echo默认输出了换行，于是需要开启-n去掉换行

```
echo -ne "foo"
echo -ne "\rbar"
```

这样将会产生正确输出，但是这样太麻烦了，有些时候\b太多，有些时候甚至不能确定有多少个\b

于是可以使用\r，它的作用是使当前光标回到当前行首

```
echo -ne "foo"
echo -ne "\rbar"
```

这样就可以了。但是当每次记录长度不一样时还是会出错，比如

```
echo -ne "barbar"
echo -ne "\rfoo"
```

这样会输出foobar而不是foo

后来查阅资料说可以使用[terminalcodes](http://wiki.bash-hackers.org/scripting/terminalcodes)

```
echo -ne "barbar"
echo -ne "\033[1K\rfoo"
```

即首先清空行首到当前位置的内容，然后光标回行首，再输出foo

利用以上说的便可以实现很多linux程序等待时出现的`/|\-`旋转效果

{% codeblock rotate.sh lang:bash %}
#!/bin/bash

function rotate {
	cnt=0
	while true; do
		case $cnt in
			0) echo -ne "|\r";;
			1) echo -ne "/\r";;
			2) echo -ne "-\r";;
			3) echo -ne "\\r";;
			*) exit -1;;
		esac
		sleep 0.2
		(( cnt=(cnt+1)%4 ))
	done
}

rotate
{% endcodeblock %}
