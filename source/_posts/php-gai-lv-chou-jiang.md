---
layout: post
title:  "php概率抽奖算法"
date:   2016-11-21 11:54:34 +0800
tags:   PHP
---

```php
<?php
/**
 * bingo
 * 
 * @param float $probability 概率 例如：0.01
 *
 * @return bool
 */
function bingo($probability)
{
    $denominator = (int)(1/$probability);
    $numerator = (int)($probability*$denominator);
    $chance = rand(1, $denominator);
    return $chance <= $numerator;
}
```

使用示例：  
```php
<?php
$prizes = array(
    array('id' => 1, 'name' => '小米手机', 'probability' => '0.009'),
    array('id' => 2, 'name' => 'iPhone 6s', 'probability' => '0.0001'),
    array('id' => 3, 'name' => '优惠券300', 'probability' => '0.002'),
    array('id' => 4, 'name' => '16G优盘', 'probability' => '0.01'),
);
while(true) {
    shuffle($prizes);
    foreach($prizes as $prize) {
        $result = false;
        if ($this->bingo($prize['probability'])) {
            $result = $prize;
            break;
        }
    }
    if($result) {
        break;
    }
}
print_r($result);
```