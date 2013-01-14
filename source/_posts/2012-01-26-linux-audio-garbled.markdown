---
layout: post
title: "Linux音频乱码"
date: 2012-01-26 20:20
comments: true
categories: ["linux"]
---

mp3文件采用gbk编码，而linux使用utf8，所以会造成混乱

使用python-mutagen可以解决

```sh
mid3iconv -e gbk *.mp3
```

详细参考：
[http://yp.oss.org.cn/software/show_resource.php?resource_id=313](http://yp.oss.org.cn/software/show_resource.php?resource_id=313)
