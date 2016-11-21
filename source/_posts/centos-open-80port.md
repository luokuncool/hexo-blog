---
layout: post
title:  "CentOS下打开关闭80端口"
date:   2016-11-21 23:29:34 +0800
tags: 
  - linux 
  - centos
---

1、打开端口  

`iptables -I INPUT -ptcp --dport  80 -j ACCEPT`

2、关闭端口  

`iptables -I INPUT -p tcp --dport 80 -j DROP`

注：执行完后用service iptables save保存一下，避免服务器重启后失效