---
layout: post
title: "start ibus at boot"
date: 2012-01-25 19:59
comments: true
categories: ["linux", "ibus"]
---

backup

~/.profile

```sh
# ibus start at boot
export XMODIFIERS=@im=ibus
export GTK_IM_MODULE=ibus
export QT_IM_MODULE=ibus
ibus-daemon -d -x
```
