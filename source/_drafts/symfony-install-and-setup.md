---
title: 安装及设置Symfony
tags:
  - PHP
  - Symfony
---

> 英文原文地址：http://symfony.com/doc/current/setup.html

这篇文章介绍怎样用不同的方法来安装Symfony，以及解决在安装过程中出现的最基本的问题。

> 如果你喜欢视频教程的话，[戳这里](http://knpuniversity.com/screencast/symfony)

## 创建Symfony应用

Symfony提供了一个叫`Symfony Installer`的专门应用来简化Symfony应用的创建。这个安装程序兼容PHP5.4，只需要在你的系统上安装一次就可以了。

```bash
# Linux and macOS systems
$ sudo mkdir -p /usr/local/bin
$ sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony
$ sudo chmod a+x /usr/local/bin/symfony

# Windows systems
c:\> php -r "readfile('https://symfony.com/installer');" > symfony
```

> 在Linux和macOS下，通过上面的操作一个全局的命令`symfony`就创建起了。
> 在Windows下， 把`symfony`这个文件放到一个在`PATH`环境变量的目录中来创建一个全局的命令，或者把它移动到其它的对你方便的目录。

```bash
# for example, if WAMP is used ...
c:\> move symfony c:\wamp\bin\php
# ... then, execute the command as:
c:\> symfony

# moving it to your projects folder ...
c:\> move symfony c:\projects
# ... then, execute the command as
c:\> cd projects
c:\projects\> php symfony 
```

一旦安装好了`Symfony Installer`， 就可以使用`new`创建Symfony应用了：

```bash
$ symfony new my_project_name
```

如上命令会创建一个叫`my_project_name`的目录，该目录包含一个基于最近的symfony稳定版本创建的空项目。这个安装程序会检测你的系统是否具备运行Symfony应用的环境。

如果没有，你会看到需要做那些配置来满足运行的需求。

> 如果安装程序不能正常工作或者没有输出任何东西，请确认你是否安装并启用了[Phar extension](http://php.net/manual/zh/intro.phar.php)

## 用指定的symfony版本来创建项目

如果你的项目需要基于指定的symfony版本来创建，请使用`new`命令的第二个可选参数：

```bash
# use the most recent version in any Symfony branch
$ symfony new my_project_name 2.8
$ symfony new my_project_name 3.1

# use a specific Symfony version
$ symfony new my_project_name 2.8.3
$ symfony new my_project_name 3.1.5

# use a beta or RC version (useful for testing new Symfony versions)
$ symfony new my_project 2.7.0-BETA1
$ symfony new my_project 2.7.0-RC1

# use the most recent 'lts' version (Long Term Support version)
$ symfony new my_project_name lts
```

每个版本都有自己的文档，你可以选择任一文档页面。

> 阅读[Symfony发布过程](http://symfony.com/doc/current/contributing/community/releases.html)来更好的理解为什么会有好几个Symfony版本，以及该选择哪个版本用到你的项目中

## 使用Composer创建Symfony项目

如果你使用的是PHP5.3或者因为其他任何原因不能够使用`Symfony instaler`，你可以使用[Composer](http://docs.phpcomposer.com/)来创建Symfony项目，`composer`是一个被好多时髦php应用所使用的依赖管理器。

如果你的电脑没有安装Composer，看这篇文章[安装Composer](/2016/11/25/install-composer/)。然后执行`create-project`命令来创建一个基于最新稳定版的Symfony的项目：

```bash
$ composer create-project symfony/framework-standard-edition my_project_name
```

当然你也可以通过传第二个参数给`create-project`来安装其他Symfony版本：

```bash
$ composer create-project symfony/framework-standard-edition my_project_name "2.8.*"
```

> 如果你的网络比较慢的话，你可能会以为Composer没有在工作。如果你是这样的，添加`-vvv`到以上命令来展示Composer正在做的详情。

## 运行Symfony应用

开发阶段Symfony借助php内部的web服务器(自php5.4开始支持)来运行应用。因此，运行Symfony应用只需要进入项目目录然后执行这个命令：

```bash
$ cd my_project_name/
$ php bin/console server:run
```

然后，打开你的浏览器访问`http://localhost:8000`地址看到Symfony的欢迎页面：

![](/images/20161130/welcome.png)

如果你没有看到欢迎页而是看到了空白页或者错误页，或许是目录权限没对。

## 检查Symfony应用的配置和设置

Symfony安装程序可以检测你的系统能否运行Symfony应用。但是，php的命令行运行的配置有可能和php的web运行配置不一样。为此，Symfony提供了一个配置检测的界面。访问下面的链接来检测你的配置并且在继续前修复所有问题。

```
http://localhost:8000/config.php
```

## 修复权限问题

如果你有任何文件权限错误或者看到白屏，可以阅读[设置或修复文件权限问题](https://symfony.com/doc/3.4/setup/file_permissions.html)获取更多信息

## 更新Symfony应用

这点的前提是你已经创建了一个完整功能的symfony应用。每个symfony应用都会依赖一些第三方的类库。这些类库存储在 `vendor` 目录下面并由composer来管理。  

经常更新这些类库是避免bug和安全漏洞的好方法。执行`update` Composer命令来立即更新他们（这个操作花几分钟取决于你项目的复杂程度）：

```bash
$ cd my_project_name/
$ composer update
```

> Symfony还提供了一个命令来检测你的项目是否存在已知的安全漏洞：
> `$ php bin/console security:check`
> 经常执行这个命令能尽快的更新或替换受损坏的依赖包。

## 安装symfony例子或其它分发包

你已经下载了[Symfony Standard Edition](https://github.com/symfony/symfony-standard)：用来开发symfony应用的默认项目。你将在整个文档中使用此项目来构建您的应用程序。

Symfony还提供了一些其它项目和初始架构供你使用：

[The Symfony Demo Application](https://github.com/symfony/demo)