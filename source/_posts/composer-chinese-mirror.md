---
layout: post
title:  "composer 中国镜像"
date:   2016-11-21 15:16:34 +0800
tags:
  - php 
  - composer
---

打开命令行输入如下命令  
`composer config repo.packagist composer http://packagist.phpcomposer.com`  

然后输入如下命令查看配置信息  
`composer config -l|grep url`

显示如下信息，配置成功  
`[repositories.packagist.url] http://packagist.phpcomposer.com`

