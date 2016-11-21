---
layout: post
title:  "php用curl上传文件"
date:   2016-11-21 22:55:34 +0800rl
tags:
  - php
  - curl
---

这两天写一个接口需要接收curl传过来的文件，测试的时候接受端代码如下：

    print_r($_FILES);    

发送端代码如下：
```php
<?php
$ch = curl_init('http://example.com/upload.php');
$data = array('test_file' => '@/path/to/file');
curl_setopt($ch, CURLOPT_POST,1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
curl_exec($ch);
```
照理说应该打印出$\_FILES对应值，但是$\_FILES始终是一个空数组，纠结了半天，后来搜到在php5.5及以后已经弃用了使用@符号上传文件了，需要使用[curl\_file\_create](http://php.net/curl\_file\_create)函数来创建上传文件，示例代码如下：
```php
<?php
$ch = curl_init('http://example.com/upload.php');
$cfile = curl_file_create('cats.jpg','image/jpeg','test_name');
$data = array('test_file' => $cfile);
curl_setopt($ch, CURLOPT_POST,1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
curl_exec($ch);
```