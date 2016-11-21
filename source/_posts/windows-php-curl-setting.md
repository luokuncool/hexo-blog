---
layout: post
title:  "windows下php启用curl扩展"
date:   2016-11-21 16:28:34 +0800
tags: 
  - php
  - curl
---

今天用apache跑php的时候发现没有curl扩展  
我把`;extension=php_curl.dll`前面的分号去掉然后重启apache服务，so easy.  
刷新页面，我去！怎么还是没有？  
原来要把php目录下`ssleay32.dll`、`libeay32.dll`、`libssh2.dll`复制到system32下，复制过去，这下好了  
参考：
http://php.net/manual/zh/curl.installation.php
