---
layout: post
title: "shell script while cmd output"
date: 2012-05-23 15:18
comments: true
categories: ["shell"]
---

zz《精通UNIX shell脚本编程》
这是节省CPU周期和磁盘I/O时间的一个小技巧，而且不受管道的2048个字符数的限制

```sh
while read LINE
do
    echo "$LINE"
    :
done < <(command)
```

这个方法看起来有点奇特。这里所作的就是在done循环中止符之后，从循环底部进行输入重定向，用done<表示。<(command)符号执行命令，并把命令的输出结果指向循环底部。
注意：在< <(command)中的两个< <之间必须有空格


