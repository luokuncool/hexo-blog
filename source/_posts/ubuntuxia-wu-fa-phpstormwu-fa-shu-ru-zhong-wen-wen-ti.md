---
layout: post
title:  "ubuntu下无法phpstorm无法输入中文问题"
date:   2016-11-21 21:50:34 +0800
tags: phpstrom
---

首先在`~/.xinitrc`里面加入以下代码：
```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```
然后在phpstorm.sh的顶部加上以下代码：
```
XMODIFIERS="@im=fcitx"
export XMODIFIERS
```
再重启phpstorm就可以输入中文啦