---
title: 安装Composer
tags:
  - PHP
  - Composer
date: 2016-12-01 17:30:09
---

> 原文地址：[http://docs.phpcomposer.com/00-intro.html#System-Requirements](http://docs.phpcomposer.com/00-intro.html#System-Requirements)

# 系统要求

运行 Composer 需要 PHP 5.3.2+ 以上版本。一些敏感的 PHP 设置和编译标志也是必须的，但对于任何不兼容项安装程序都会抛出警告。

我们将从包的来源直接安装，而不是简单的下载 zip 文件，你需要 git 、 svn 或者 hg ，这取决于你载入的包所使用的版本管理系统。

Composer 是多平台的，我们努力使它在 Windows 、 Linux 以及 OSX 平台上运行的同样出色。

# 安装 - *nix

## 下载 Composer 的可执行文件

### 局部安装

要真正获取 Composer，我们需要做两件事。首先安装 Composer （同样的，这意味着它将下载到你的项目中）：

```bash
curl -sS https://getcomposer.org/installer | php
```

> 注意： 如果上述方法由于某些原因失败了，你还可以通过 `php` >下载安装器：

```bash
php -r "readfile('https://getcomposer.org/installer');" | php
```

这将检查一些 PHP 的设置，然后下载 composer.phar 到你的工作目录中。这是 Composer 的二进制文件。这是一个 PHAR 包（PHP 的归档），这是 PHP 的归档格式可以帮助用户在命令行中执行一些操作。

你可以通过 --install-dir 选项指定 Composer 的安装目录（它可以是一个绝对或相对路径）：

```bash
curl -sS https://getcomposer.org/installer | php -- --install-dir=bin
```

### 全局安装

你可以将此文件放在任何地方。如果你把它放在系统的 PATH 目录中，你就能在全局访问它。 在类Unix系统中，你甚至可以在使用时不加`php`前缀。

你可以执行这些命令让 `composer` 在你的系统中进行全局调用：

```bash
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```

> 注意： 如果上诉命令因为权限执行失败， 请使用 sudo 再次尝试运行 `mv` 那行命令。

现在只需要运行 `composer` 命令就可以使用 Composer 而不需要输入 `php composer.phar`。

### 全局安装 (on OSX via homebrew)

Composer 是 homebrew-php 项目的一部分。

```bash
brew update
brew tap josegonzalez/homebrew-php
brew tap homebrew/versions
brew install php55-intl
brew install josegonzalez/php/composer
```

# 安装 - Windows

## 使用安装程序

这是将 Composer 安装在你机器上的最简单的方法。

下载并且运行 [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe)，它将安装最新版本的 Composer ，并设置好系统的环境变量，因此你可以在任何目录下直接使用 `composer` 命令。

## 手动安装

设置系统的环境变量 `PATH` 并运行安装命令下载 `composer.phar` 文件：

```bash
C:\Users\username>cd C:\bin
C:\bin>php -r "readfile('https://getcomposer.org/installer');" | php
```

> 注意： 如果收到 readfile 错误提示，请使用 `http` 链接或者在 php.ini 中开启 `php_openssl.dll` 。

在 `composer.phar` 同级目录下新建文件 `composer.bat` ：

```bash
C:\bin>echo @php "%~dp0composer.phar" %*>composer.bat
```

关闭当前的命令行窗口，打开新的命令行窗口进行测试：

```bash
C:\Users\username>composer -V
Composer version 27d8904
```