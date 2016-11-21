---
layout: post
title:  "几个面试题"
date:   2016-11-21 00:26:34 +0800i
tags: 
  - php
  - mysql
---

1、编写一个函数将1,2,3,4,5,6随机放到一个数组,3不能在第三的位置，5和6不能挨着。

```php
<?php
function sort_test($array)
{
    while (true) {
        shuffle($array);
        $temp = array_flip($array);
        if ($array[2] != 3 && 1 != abs($temp[5] - $temp[6])) {
            return $array;
        }
    }
}

$array = array(1, 2, 3, 4, 5, 6);
print_r(sort_test($array));
```

2、编写一个函数实现 将字符串$str="asdfasflasdfopafdsa", 中第一个之出现过一次的字母。

```php
<?php
function get_target_letter($str)
{
    $i     = 0;
    $array = array();
    while (isset($str[$i])) {
        $array[$str[$i]] = isset($array[$str[$i]]) ? $array[$str[$i]] + 1 : 1;
        $i++;
    }
    foreach ($array as $key => $val) {
        if ($val == 1) {
            return $key;
        }
    }
    return false;
}

echo get_target_letter('asdfastflasdfopafdsa');
```

3、有两个表，请编写一条sql语句查询出t1-t2时间段间的产品名、销售量总数并按照销售量总数由高到低排序。

```mysql
CREATE TABLE products (
  product_id INT UNSIGNED NOT NULL PRIMARY KEY AUTO_INCREMENT,
  product_name VARCHAR(64) NOT NULL
);
CREATE TABLE orders (
  product_id INT UNSIGNED NOT NULL ,
  create_at INT UNSIGNED NOT NULL,
  num INT UNSIGNED NOT NULL 
);
```

我的解法:  

```mysql
SELECT
  products.product_name,
  number.num
FROM ((SELECT
     product_id,
     sum(num) AS num
       FROM orders
     WHERE orders.create_at BETWEEN {$t1} AND {$t2}
       GROUP BY product_id) AS number) INNER JOIN products ON products.product_id = number.product_id
ORDER BY number.num DESC;
```