---
layout: post
title:  "CentOS下php安装mcrypt扩展"
date:   2016-11-21 17:36:34 +0800
tags: 
  - linux 
  - centos
---


#### 1、 先去 http://www.sourceforge.net 下载Libmcrypt,mhash,mcrypt安装包。

#### 2、 然后分别安装，都是解压后，进入源码目录执行以下代码。
    
    ./configure
    make && make install  
    
#### 3、 再去php.net下载对应版本的php源码，然后安装mcrypt模块即可，如下以5.3.3为例  

    tar zxvf php-5.3.3.tar.gz  
    cd php-5.3.3/ext/mcrypt
    phpize
    ./configure --with-php-config=/usr/bin/php-config
    make && make install
    
安装完成以后，再去php配置文件里引入指定的扩展即可。   
注：这里的php-config有可能不在/usr/bin目录，可以先用whereis php-config查看一下php-config在哪里。  

参考网址：http://www.cnblogs.com/huangzhen/archive/2012/09/12/2681861.html