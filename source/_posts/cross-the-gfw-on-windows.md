---
title: Windows科学上网
tags:
  - ShadowSocks
  - 科学上网
date: 2016-11-24 18:27:19
---


## 前言

干技术的同行都知道，工作中遇到的好多问题都能通过Google搜索到解决方案。  

但是比较坑爹的是由于gfw的存在，大陆根本访问不到Google。 

但是这根本困不住我们的技术大牛些， 他们给自己开发了个可以穿过gfw的软件，也就是ShadowSocks(以下简称SS)。这个软件分为服务端和客户端，我们要用的大多数情况下只是客户端。

## 安装配置

### SS账号

SS账号有两个途径获得，一个嘛就是自己搭建一个SS服务器，然后自然而然就有账号了。要搭建SS服务器必须要有个自己的非大陆的服务器，然后在上面安装SS的服务端。这个嘛并不建议这么干，因为组个服务器贵啊，建SS太浪费了，虽说有便宜的，但是便宜的速度又不行。

另一个就是在服务商那里买个SS账号，我买的一个账号120一年，一个月30G流量，包括20多台服务器节点，不过我只用了一台香港的，速度飞快，看YouTube的720p视频无压力。

至于SS服务商这里就不贴连接出来了，因为老是被封，换了n个域名了，自己去百度搜索吧。

### SS客户端安装

点[下载链接](https://github.com/shadowsocks/shadowsocks-windows/releases)下载SS客户端，下载下来解压启动就是了。

如果启动不起，基本上就是没有安装[.net framework 4.0][.net framework 4.0]再去下载个[.net framework 4.0][.net framework 4.0]安装好就可以了。

![ss配置](/images/20161124174335.png)

启动起来以后把ss账号的相关信息填写到客户端里面，就可以了。

### 安装SwitchyOmega插件

点击[下载链接][SwitchyOmega]下载SwitchyOmega。

下载`SwitchyOmega.crx`，别下载到`tar.gz`, `zip`了，那个是源码。

如果是用chrome下载的，会直接提示安装，如果是其它下载的则打开chrome打开菜单`更多工具`->`扩展程序`， 然后用鼠标把`SwitchyOmega.crx`拖到chrome里面就会安装了。

![](/images/20161124181041.png)

> 下面所有的设置记住要点左下角那个应用选项哈

安装好以后点chrome右上角那个小圈圈，进入到设置页面。情景模式设置一个shadowsocks把服务器设置为localhost，协议socks5，端口1080

![](/images/20161124175527.png)

然后再点那个圆圈圈选择刚刚添加的shadowsocks情景模式就可以，上谷歌了。

但是~ 现在浏览网页所有的网站都会走代理，包括国内的网站，这些网站走代理其实要慢得多。

所以~ 看到截图那里那个auto switch 了吧，这个就是让你想走代理的网站走代理，不走的就直接连接

![](/images/20161124181639.png)

现在再来新增一个情景模式，类型选择自动切换，设置如下红框圈出来的地方。

规则列表网址`https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt`, 填了规则列表网址点一下立即更新情景模式。

![](/images/20161124182136.png)

这个新增好以后，又点击右上角的小圈圈，把情景模式设置成`auto switch`，现在浏览网页的时候大多数被墙了的网站都会走代理了。当遇到访问比较慢的没有被墙的国外网站，也可以点击右上角的小圈圈，然后点`添加规则`，情景模式选择`shadowsocks`，然后访问这个网址就是走代理了，速度飞起来。

![](/images/2016-11-24 20-36-14.png)

### 安装Proxifier

SS账号安装好了，浏览器是能顺利的翻墙上网了。

但是做技术的朋友们经常会用的命令行，比如[composer](http://docs.phpcomposer.com/), [npm](https://docs.npmjs.com/)之类的依赖管理工具。

在使用这些工具安装依赖包的时候回遇到有些依赖库由于网络原因安装不起，虽然可以把镜像设置为国内的镜像，但是个人觉得并不是太靠谱，有的还是下载不下来。这时候就需要安装`proxifier`来让这些工具也走代理。

点击[下载链接](https://www.baidu.com/s?wd=Proxifier&rsv_spt=1&rsv_iqid=0xac11c3ce0002b131&issp=1&f=8&rsv_bp=0&rsv_idx=2&ie=utf-8&tn=baiduhome_pg&rsv_enter=1&rsv_sug3=3&rsv_n=2)把`proxifier`下载下来之后，一步步安装起，接着启动。

![proxifier配置](/images/20161124163600.png)

点菜单`配置文件`->`代理服务器`->`添加`, 地址填localhost，端口填1080(默认是这个和上面那个SS本地端口一致)，类型选`SOCKS5`。

![](/images/20161124165012.png)

服务器配置好以后，再配置代理规则，点菜单`配置文件`->`代理规则`，规则列表里面默认有一条default, 把default规则那一栏设置成刚刚配置的那个代理服务器。

## 开始穿墙

* [Google](https://www.google.com.hk)
* [YouTube](https://www.youtube.com/?gl=HK)


[.net framework 4.0]: https://www.baidu.com/s?wd=.net%20framework%204.0&rsv_spt=1&rsv_iqid=0x8455a7f400005761&issp=1&f=8&rsv_bp=1&rsv_idx=2&ie=utf-8&rqlang=cn&tn=baiduhome_pg&rsv_enter=1&oq=npm&rsv_t=2439rIAKIwx4rky9PNtLB4DMlZoQ9N8ocsYGT3SMGTJfvdmawCrpVCWTlllVvs0sZXlX&inputT=1588&rsv_pq=bcf76b02000066a4&rsv_sug3=11&rsv_sug1=6&rsv_sug7=100&bs=npm

[SwitchyOmega]: https://github.com/FelisCatus/SwitchyOmega/releases
