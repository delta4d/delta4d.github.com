---
layout: post
title: "ibus-pinyin can not set preference"
date: 2013-02-04 01:38
comments: true
categories: ["ibus"]
---

在ibus-setup的时候选择ibus-pinyin的首选项可能会出现‘ImportError: No module named gtk’，这是由于默认python会指向python3，而不是python2。python的版本引出了多少问题啊。。

这时只要指定相关文件的python版本就可以了

执行`locate ibus-setup-pinyin`找到文件位置，然后将python替换成python2即可

下面是一个简单的脚本

{% codeblock ibus-setup-pinyin-fix lang:sh%}
#!/usr/bin/env sh

[[ $EUID -ne 0 ]] && {
	echo "Run this with ROOT!"
	exit
}

read -p "This script should be executed only one time, \
r u sure to continue ? [Y/N] " IN

[[ "$IN" != "Y" ]] && exit

FILE="`locate ibus-setup-pinyin`"

cp "$FILE" "$FILE-bak"
cat "$FILE-bak" | sed 's/python/python2/g' > "$FILE"
echo "the original file was backed up to $FILE-bak"
{% endcodeblock %}
