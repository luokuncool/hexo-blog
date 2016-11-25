---
layout: post
title:  "centos下安装mysql"
date:   2015-08-05 15:05:34 +0800
tags: 
  - Linux
  - MySQL
---

* MySQL的编译安装会花费较长的时间，所以我喜欢采用了懒人方法。  

```bash
yum install -y mysql-server
```

* 安装完成后启动

```bash
service mysqld start
```

* 登陆到mysql，修改密码, 安装完成后是没有密码的

```
mysql -uroot -p
update mysql.user set password=password('root');
flush privileges;
```

* 退出以后就可以用新的密码登陆了

```bash
mysql -uroot -proot
```