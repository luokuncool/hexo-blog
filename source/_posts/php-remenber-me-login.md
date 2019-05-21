---
layout: post
title:  "关于php记住帐号免登陆功能"
date:   2016-11-21 21:50:34 +0800
tags:   PHP
---

之前一直没有搞明白记住登陆功能问题, 在开发php应用的时候每次关闭浏览器登录状态就失效了，想要做一个一定时间内免登录的功能。  

其实要实现这个功能其实非常简单，只需要设置
[session.cookie_lifetime](http://php.net/manual/zh/session.configuration.php#ini.session.cookie-lifetime)的值就可以了，例如
`ini_set('session.cookie_lifetime', 3600*24*30);`可以将PHPSESSID这个cookie有效期保持三十天。

`session.cookie_lifetime`以秒数指定了发送到浏览器的 `cookie` 的生命周期。值为 0 表示“直到关闭浏览器”。默认为 0。当其设置为0的时候关闭浏览器就过期了，再打开浏览器就需要重新登录。在程序里面用`ini_set('session.cookie_lifetime', 3600*24*30);`设置时间就可以控制登录状态保持的时间了。