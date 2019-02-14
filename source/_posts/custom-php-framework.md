---
title: 嗯！我自己写（东拼西凑）了个"框架"
date:   2019-02-14 17:00:34 +0800
tags:
  - PHP
  - Doctrine
  - PHP-DI
---

## 为什么要自己「写」框架？

我们都知道在 `github` 有很多优秀的开源项目，有的项目是专注于某一项事情的库。比如某某项目是一个 HTTP 客户端，某某项目是一个处理图片的库，某某项目是一个模板引擎等等。
它们在自己所处领域都是非常优秀的，于是我奔着学习使用这些库的目的，挑选自己喜欢的把他们组成一个简易的自定义框架。

## 框架的构成

一个完善的框架有很多特性，现在我准备取一些基础必须的来一个个拆解，并选择我喜欢用的扩展包。
我这次组装的框架将会包括如下特性：
* IOC容器
* 数据库访问
* 路由
* 日志
* 模板引擎
* 配置文件加载
* 调试

## 元件组装

### IOC容器`php-di/php-di`

框架存在的目的是为了让我们更方便的来开发我们的项目，让我们更容易的写出松耦合便于维护的代码。要起到解耦合作用框架常常是采用的一个依赖注入容器来处理。不管是大名鼎鼎的 `spring` 还是我们的 `laravel` 都是这样。所以我来到 [packagist.org](https://packagist.org/) 找对应的扩展包。分别看了排名前几个的最终挑选了 `php-di` 。它吸引的地方是容易上手，并且可以通过注解的方式来注入对象，非常方便。

![php-di](https://luokuncool.pw/images/20190124/100048.png)

* 创建容器

```php
<?php

AnnotationRegistry::registerLoader(array($loader, "loadClass"));
$builder = new ContainerBuilder();
$builder->useAnnotations(true);
$container = $builder->build();

//bootstrap.php
```

* 通过注解注入对象

```php
<?php


namespace App\Controller;


use App\Repository\SettingRepo;
use DI\Annotation\Inject;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\Response;

class SettingController
{
    /**
     * @Inject()
     * @var SettingRepo
     */
    private $settingRepo;

    public function set($params)
    {
        $this->settingRepo->set($params['key'], $params['value']);
        return new Response("Set \"{$params['key']}\" value to \"{$params['value']}\"");
    }

    public function get($params)
    {
        return new Response(app(SettingRepo::class)->get($params['key']));
    }

    public function all()
    {
        return new JsonResponse($this->settingRepo->all());
    }
}

```

可以看到我并没有对 `SettingController` 的 `settingRepo` 属性做任何赋值，我只是给他加了 `@Inject()` 注解，我就可以使用它了，是不是很方便。

### 数据库操作`doctrine/dbal`

要开发项目必然要存取数据，数据库操作免不了，又来到 [packagist.org](https://packagist.org/) 找到了强大的 [doctrine/dbal](https://github.com/doctrine/dbal)

![doctrine-dbal](https://luokuncool.pw/images/20190124/100357.png)

* 先在容器中加入一个数据库连接

```php
<?php

$builder->addDefinitions([
    'db'     => function (\DI\Container $c) {
        $config           = $c->get('config');
        $connectionParams = ['url' => $config['db.url']];

        $configuration = new Configuration();
        $configuration->setSQLLogger($c->get(\App\Doctrine\DBAL\Logging\QueryFileLogger::class));

        return DriverManager::getConnection($connectionParams, $configuration);
    }
]);
```

* 操作数据库

```php
<?php

namespace App\Repository;

use DI\Annotation\Inject;
use Doctrine\DBAL\Connection;

class SettingRepo
{
    /**
     * @Inject("db")
     * @var Connection
     */
    private $db;

    public function get($key)
    {
        $result = $this->db->createQueryBuilder()->select('value')
            ->from('setting')
            ->where("key = :key")
            ->setParameter('key', $key)
            ->setMaxResults(1)
            ->execute()->fetch();
        return $result['value'];
    }

    public function set($key, $value, $description = '')
    {
        if ($this->get($key)) {
            $this->db->update('setting', ['value' => $value], ['key' => $key]);
        } else {
            $this->db->insert('setting', ['key' => $key, 'value' => $value, 'description' => $description]);
        }
    }

    public function all()
    {
        return $this->db->createQueryBuilder()->select('*')
            ->from('setting')
            ->execute()->fetchAll();
    }
}
```

### 路由symfony/routing

和以上操作一样，最终挑选了 `symfony/routing` 作为路由组件。

```php
<?php

$routes = new RouteCollection();
$routes->add('home', new Route("/", ['_controller' => [\App\Controller\WelcomeController::class, 'home']]));

$request    = \Symfony\Component\HttpFoundation\Request::createFromGlobals();
$context    = new RequestContext();
$matcher    = new UrlMatcher($routes, $context);
$parameters = $matcher->matchRequest($request);

$controller = $parameters['_controller'];
if ($controller instanceof Closure) {
    $response = $controller($parameters);
} else {
    $response = app($controller[0])->{$controller[1]}($parameters);
}
$response->send();
```

访问首页最终看到 "Welcome!" ，路由成功。

### 其它组件

其它组件都是按照如上操作一个个加入进来的就不一一介绍详细介绍了。

* 日志
开发过程中常常会记录日志，最终选用monolog来记录日志。

* 模板引擎
模板引擎必不可少，选用twig

* 配置文件加载
使用 [hassankhan/config](https://github.com/hassankhan/config)

* 调试
写代码肯定会时不时写错，默认的报错信息不方便跟踪，选用 `symfony/debug`, `symfony/var-dumper` 调试。

## 小结

通过一步步的加入各个扩展包可以帮助学习对应的扩展包。也可以更深入的理解到框架都由哪些构成。[这里](https://github.com/luokuncool/custom-framework)是例子代码。此代码只是做抛砖引玉的作用，感兴趣的朋友可以根据自己的喜好，加入自己喜欢用的扩展包来实现想要的功能。
