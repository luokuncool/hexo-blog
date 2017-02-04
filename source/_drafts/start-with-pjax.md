---
title: 使用pjax创建更优的用户体验
tags:
  - PHP
  - JavaScript
---

## 前言

之前在[laravel-china](http://laravel-china.org)看到一个让我眼前一亮的一个交互功能。

当点击超链接的时候，发送的是一个ajax请求，响应的是网页一部分内容并把响应内容填充到html文档内。

到此为止都很好理解，就是ajax的常规用法，但是据我所知道的，重新刷新这个页面的时候又会回到上一页去，因为ajax请求并不会改变地址栏的url地址。

但是当我刷新的时候页面还是停留在我点击链接后的那个界面，这时候我才发现地址栏的地址其实已经改变了，并且刷新的时候响应内容是一个完整的html文档。



![](/images/20170203125468.gif)

https://github.com/luokuncool/hexo-theme-next/blob/master/layout/_scripts/pjax.swig

```php
<?php
namespace Blog\Controller;

use DI\Annotation\Inject;
use Symfony\Component\HttpFoundation\Response;

abstract class BaseController
{
    /**
     * @Inject("twig")
     * @var \Twig_Environment
     */
    protected $twig;

    /**
     * Renders a template.
     *
     * @param string $name    The template name
     * @param array  $context An array of parameters to pass to the template
     *
     * @param bool   $pjax
     *
     * @return Response The rendered template
     *
     */
    public function render($name, array $context = array(), $pjax = false)
    {
        $response = new Response();
        if (!$pjax) {
            return $response->setContent($this->twig->render($name, $context));
        }

        $template = $this->twig->load($name);
        foreach ($template->getBlockNames($context) as $blockName) {
            $block = $template->renderBlock($blockName, $context);
            $response->setContent($response->getContent() . ($blockName == 'title' ? "<title>$block</title>" : $block) . PHP_EOL);
        }
        return $response;
    }
}
```



|     name     | age |             blog                |
| ------------ | --- | ------------------------------- |
| _LearnShare_ |  12 | [LearnShare](http://xianbai.me) |
| __Mike__     |  32 | [Mike](http://mike.me)          |


D:\dev\php-5.6.19-Win32-VC11-x86\php.exe D:\www\di-demo\bin\console