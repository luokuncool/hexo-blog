---
title: PHP位运算使用
tags:
  - PHP
date: 2019-01-11 18:10:50
---


## 位运算符 

如果你正准备看下去，你应该先搞懂各个位运算符的作用。 以下是官网的一个介绍。

| 例子             | 名称            |  结果 |
| -------------   | -------------   | ---- |
| $a & $b         | And（按位与）     | 将把 $a 和 $b 中都为 1 的位设为 1。 |
| $a &verbar; $b  | Or（按位或）      | 将把 $a 和 $b 中任何一个为 1 的位设为 1。 |
| $a ^ $b         | Xor（按位异或）    | 将把 $a 和 $b 中一个为 1 另一个为 0 的位设为 1。 |
| ~ $a            | Not（按位取反）    | 将 $a 中为 0 的位设为 1，反之亦然。 |
| $a << $b        | Shift left（左移） | 将 $a 中的位向左移动 $b 次（每一次移动都表示“乘以 2”）。 |
| $a >> $b        | Shift right（右移）| 将 $a 中的位向右移动 $b 次（每一次移动都表示“除以 2”）。 |

详情请点击【[这里](http://php.net/manual/zh/language.operators.bitwise.php)】了解。

## 平常开发需要用位运算吗？

之前我一直以为对我平常开发来说我并不需要用到位运算符，我觉得这东西需要做很复杂的操作才会用到。  
但是在我用了很多次`json_encode($array, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT)`这个函数以后，我开始好奇为什么只用一个参数就可以控制json字符串输出的两个设置，既能格式化输出还能不把内容编码成`\uXXXX`可以更直观的看到中文是什么内容。

于是我分别打印了`JSON_UNESCAPED_UNICODE`和`JSON_PRETTY_PRINT`的值，他们分别是`256`和`128`，开始十进制开不出个所以然。于是我对比了他们的二级制值。

| JSON_UNESCAPED_UNICODE  | JSON_PRETTY_PRINT   |  JSON_UNESCAPED_UNICODE &verbar; JSON_PRETTY_PRINT |
| -------------           | ------------------- | ------------------------------------------------  |
| 256                     | 128                 | 384  |
| 0b100000000             | 0b010000000         | 0b110000000  |

可以看到从左往右数`256`第8位是1，而`128`第七位是1，通过按位或运算以后7位和八位都成了1。那函数内部就可以只需要判断`json_encode`的第二个参数的二级制数第8位如果是1就是`JSON_UNESCAPED_UNICODE`为真，第7位如果是1就是`JSON_PRETTY_PRINT`为真了。

于是我又回想了还有那个地方用了未操作符，一下又想起了这个函数`error_reporting(E_ALL ^ E_WARNING ^ E_NOTICE)`，对比下二级制值。  

| E_ALL             | E_WARNING           | E_NOTICE           |  E_ALL ^ E_WARNING ^ E_NOTICE   |
| -------------     | ------------------- | -------------------|----------------------------     |
| 30719             | 2                   | 8                  | 32757                           |
| 0b111011111111111 | 0b000000000000010   | 0b000000000001000  | 0b111111111110101               |  

可以看到`E_ALL`有一位是0，因为它是除`E_STRICT`外的所有错误和警告信息。竖着直观的看下，运算以后第1位和第3位变为0了（从右往左数第0位开始），也就是说`E_WARNING`、`E_NOTICE`被排除掉了。

```
0b111011111111111 //除`E_STRICT`外的所有错误和警告信息
0b000000000000010 //警告
0b000000000001000 //提示
0b111111111110101 //除`E_STRICT`、`E_WARNING`、`E_NOTICE`外的所有错误和警告信息
```


## 很酷，一定要用！

到此忽然觉得这玩意儿太酷了，为什么试想一下。比方说我现在要实现一个函数来初始化我家里的灯。

### 错误示范

```php
<?php
function showLight($masterRoom = 0, $livingRoom = 0, $diningRoom = 0, $secondLie = 0, $kitchen = 0) {
        echo '主卧', "\t";
        echo '客厅', "\t";
        echo '餐厅', "\t";
        echo '次卧', "\t";
        echo '厨房', "\t", PHP_EOL;
        
        echo $masterRoom, "\t";
        echo $livingRoom, "\t";
        echo $diningRoom, "\t";
        echo $secondLie, "\t";
        echo $kitchen, "\t";
}

//只开主卧灯
showLight(1);
//只开厨房灯
showLight(0, 0, 0, 0, 1);
//开所有灯
showLight(1, 1, 1, 1, 1);
```
可以看到我当我需要控制厨房灯的时候却要传其它四个参数，太不科学了。

### 再来看看用了位操作以后

先声明一个灯光控制的类，用5个位来表示5盏灯的开关为1则表示开灯。

```php
<?php

class LightControl
{
    const TURN_ON_ALL = 0b11111;
    const KITCHEN     = 0b10000;
    const SECOND_LIE  = 0b01000;
    const DINING_ROOM = 0b00100;
    const LIVING_ROOM = 0b00010;
    const MASTER_ROOM = 0b00001;
    
    private $options;

    public function __construct($options = 0)
    {
        $this->options = $options;
        echo '主卧', "\t";
        echo '客厅', "\t";
        echo '餐厅', "\t";
        echo '次卧', "\t";
        echo '厨房', "\t", PHP_EOL;
    }

    public function getOptions()
    {
        return $this->options;
    }

    public function setOptions($options)
    {
        $this->options = $options;
    }

    public function showOptions()
    {
        echo self::getOption($this->options, self::MASTER_ROOM), "\t";
        echo self::getOption($this->options, self::LIVING_ROOM), "\t";
        echo self::getOption($this->options, self::DINING_ROOM), "\t";
        echo self::getOption($this->options, self::SECOND_LIE), "\t";
        echo self::getOption($this->options, self::KITCHEN);
    }

    //获取从右往左索引位置的值
    private static function getOption($options, $option)
    {
        $index = strlen(base_convert($option, 10, 2)) - 1;
        return ($options & (1 << $index)) >> $index;
    }
}
//LightControl.php
```


```php
<?php
require __DIR__ . '/LightControl.php';

$lightControl = new LightControl();
//全关
$lightControl->showOptions();
echo PHP_EOL;

//全开
$lightControl->setOptions(LightControl::TURN_ON_ALL);
$lightControl->showOptions();
echo PHP_EOL;

//排除厨房
$lightControl->setOptions(LightControl::TURN_ON_ALL ^ LightControl::KITCHEN);
$lightControl->showOptions();
echo PHP_EOL;
//echo base_convert(LightControl::TURN_ON_ALL ^ LightControl::KITCHEN, 10 ,2), PHP_EOL;
// LightControl::TURN_ON_ALL => '11111'
// LightControl::KITCHEN     => '10000'
// result                    => '01111'

//客厅和餐厅
$lightControl->setOptions(LightControl::KITCHEN | LightControl::DINING_ROOM);
$lightControl->showOptions();
echo PHP_EOL;
//echo base_convert(LightControl::KITCHEN | LightControl::DINING_ROOM, 10 ,2), PHP_EOL;
// LightControl::KITCHEN     => '10000'
// LightControl::DINING_ROOM => '00100'
// result                    => '10100'

//test.php
```

可以看到用位操作以后可以灵活的控制灯了，如果要开的灯太多可以用排除法，要开的少可以用选择法。