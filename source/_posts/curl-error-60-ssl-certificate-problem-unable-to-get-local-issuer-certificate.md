---
layout: post
title:  "cURL error 60: SSL certificate problem: unable to get local issuer certificate"
date:   2016-11-21 21:50:34 +0800
tags:
  - php
  - symfony
---

执行`symfony demo`的时候得到错误信息`cURL error 60: SSL certificate problem: unable to get local issuer certificate`  
解决办法:  

* 把这个http://curl.haxx.se/ca/cacert.pem 连接里面的内容保存到`cacert.pem`文件。
* 修改`php.ini`把`curl.cainfo = "cacert.pem的路径"`
* 再执行`symfony demo` 问题解决了