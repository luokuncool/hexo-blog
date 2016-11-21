---
layout: post
title:  "Symfony创建非共享服务"
date:   2016-11-21 21:50:34 +0800
tags:
  - php
  - symfony
---

在syfmony服务容器中，所有的服务默认都是共享的。  
也就是说每次当你获取服务的时候，你得到的是同一个实例，这是你通常想要的。但是在某些场景下，你可能每次都想获得一个新的实例。  
要每次都获取新的实例, 只要在services配置文件里面设置 `shared `为 `false `就可以了:
    
```yaml
# app/config/services.yml
services:
    app.some_not_shared_service:
        class: ...
        shared: false
        # ...
```

现在不管什么时候，调用`$container->get('app.some_not_shared_service')`或者注入这个服务，你每次都将获取一个新的实例。