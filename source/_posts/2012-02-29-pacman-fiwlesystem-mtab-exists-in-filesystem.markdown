---
layout: post
title: "[Pacman] \"filesystem: /etc/mtab exists in filesystem\""
date: 2012-02-29 11:11
comments: true
categories: ["pacman", "linux"]
---

fixed by

```sh
pacman -S filesystem --force
```

[http://www.archlinux.org/news/filesystem-upgrade-manual-intervention-required/](http://www.archlinux.org/news/filesystem-upgrade-manual-intervention-required/)
