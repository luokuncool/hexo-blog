---
layout: post
title:  "部署NodeJS Web应用"
date:   2016-11-21 23:29:34 +0800
tags: nodejs
---

以下所说的环境都是在CentOS下，并且已经安装好NodeJs以及Apache.  
NodeJS开发的应用，在用node /path/to/app.js启动起来以后可以正常运行，但是关闭掉ssh客户端以后项目又不能访问了，这时候需要装supervisor来部署.  

1、安装及配置supervisor  
```bash
#安装
yum install supervisor
#启动
service supervisord start
```

编辑/etc/supervisord.conf 在最底部加入 

```
[program:app]
command = node /path/to/app/index.js 
directory = /path/to/app
user = app
autostart = true
autorestart = true
stdout_logfile = /var/log/supervisor/app.log
stderr_logfile = /var/log/supervisor/app_err.log
```

2、配置Apache虚拟主机
假设应用监听的端口是2368    
```
<VirtualHost *:80>
    ServerName www.test.com
    ServerAlias test.com
    ProxyRequests off
    <Proxy *>
        Order deny,allow
        Allow from all
    </Proxy>
    <Location />
        ProxyPass http://127.0.0.1:2368/
        ProxyPassReverse http://127.0.0.1:2368/
    </Location>
</VirtualHost>
```

3、最后分别启动重启下supervisord和httpd服务
```bash
service supervisord restart
service httpd restart
```