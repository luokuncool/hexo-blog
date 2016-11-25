---
title: 安装及设置Symfony
tags:
  - PHP
  - Symfony
---

> 原文地址：[http://symfony.com/doc/current/setup.html](http://symfony.com/doc/current/setup.html)

这篇文章介绍怎样用不同的方法来安装Symfony，以及解决在安装过程中出现的最基本的问题。

> 如果你喜欢视频教程的话，[戳这里](http://knpuniversity.com/screencast/symfony)

# 创建Symfony应用

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

# 用指定的symfony版本来创建项目

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

# 使用Composer创建Symfony项目

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

