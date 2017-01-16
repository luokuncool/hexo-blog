---
title: 利用redis设计抢购程序
tags:
  - PHP
  - Redis
  - MySQL
---

```mysql
CREATE TABLE goods(
  id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  title VARCHAR(255) NOT NULL COMMENT '商品名称',
  inventory INT UNSIGNED NOT NULL COMMENT '库存数量',
  PRIMARY KEY id(id)
);
```


```php
<?php
$redis = new Redis();
$redis->connect('127.0.0.1');
$inventory = $redis->decr('inventory');


```