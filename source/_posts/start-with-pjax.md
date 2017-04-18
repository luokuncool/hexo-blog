---
title: 使用pjax创建更优的用户体验
tags:
  - PHP
  - JavaScript
date: 2017-04-18 10:23:31
---


## 前言

之前在[laravel-china](http://laravel-china.org)看到一个让我眼前一亮的一个交互功能。

当点击超链接的时候，发送的是一个ajax请求，响应的是网页一部分内容并把响应内容填充到html文档内。

到此为止都很好理解，就是ajax的常规用法，但是据我所知道的，重新刷新这个页面的时候又会回到上一页去，因为ajax请求并不会改变地址栏的url地址。

但是当我刷新的时候页面还是停留在我点击链接后的那个界面，这时候我才发现地址栏的地址其实已经改变了，并且刷新的时候响应内容是一个完整的html文档。

那么这个效果是怎么实现的呢？其实最核心的就是`history.pushState`。

![](http://www.luokuncool.pw/images/20170203125468.gif)

## history.pushState介绍

在 HTML 文件中,  history.pushState() 方法向浏览器历史添加了一个状态。

pushState() 带有三个参数：一个状态对象，一个标题（现在被忽略了），以及一个可选的URL地址。下面将对这三个参数进行细致的检查：

* state object — 状态对象是一个由 pushState()方法创建的、与历史纪录相关的JS对象。当用户定向到一个新的状态时，会触发popstate事件。事件的state属性包含了历史纪录的state对象。（译者注：总而言之，它存储JSON字符串，可以用在popstate事件中。）state 对象可以是任何可以序列化的东西。由于 火狐 会将这些对象存储在用户的磁盘上，所以用户在重启浏览器之后这些state对象会恢复，我们施加一个最大640k 的字符串在state对象的序列化表示上。如果你像pushState() 方法传递了一个序列化表示大于640k 的state对象，这个方法将扔出一个异常。如果你需要更多的空间，推荐使用sessionStorage或者localStorage。
* title — 火狐浏览器现在已经忽略此参数，将来也许可能被使用。考虑到将来有可能的改变，传递一个空字符串是安全的做法。当然，你可以传递一个短标题给你要转变成的状态。（译者注：现在大多数浏览器不支持或者忽略这个参数，最好用null代替）
* URL — 这个参数提供了新历史纪录的地址。请注意，浏览器在调用pushState()方法后不会去加载这个URL，但有可能在之后会这样做，比如用户重启浏览器之后。新的URL不一定要是绝对地址，如果它是相对的，它一定是相对于当前的URL。新URL必须和当前URL在同一个源下;否则，pushState() 将丢出异常。这个参数可选，如果它没有被特别标注，会被设置为文档的当前URL。


一些情况下，调用pushState和设置 window.location = "#foo"相当，这种状况下，这两种行为都会创建和激活另一个和当前页面有关的历史纪录。但是pushState()有其他优势： 

* 新URL可以是当前URL同源下的任意地址。相反的，设置window.location会让你保持在相同页面，除非你只修改hash.
* 如果不必要，你可以不改变URL，相反的，将window.location设定为“#foo”；只会创建一个新的历史纪录，如果当前hash不为#foo.
* 你可以关联任意的数据到你的新历史纪录中。使用基于hash的方法，你需要将所有相关 的数据编码为一个短字符串。

请注意pushState()方法绝不会导致hashchange 事件被激活，即使新的URL和旧的只在hash上有区别。

### 语法
> history.pushState(state, title, url);

### 样例
创建了一个新的由 state, title, 和 url设定的浏览器历史纪录.

```javascript
var state = { 'page_id': 1, 'user_id': 5 };
var title = 'Hello World';
var url = 'hello-world.html';

history.pushState(state, title, url);
```

## 项目中搭配PHP使用

在模板中head内加入如下代码

```html
<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/nprogress/0.2.0/nprogress.min.css">
<script type="text/javascript" src="//cdn.bootcss.com/jquery/2.1.0/jquery.js"></script>
<script type="text/javascript" src="//cdn.bootcss.com/jquery.pjax/1.9.6/jquery.pjax.js"></script>
<script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/nprogress/0.2.0/nprogress.min.js"></script>
<script type="text/javascript">
$(document).on('pjax:start', NProgress.start);
$(document).on('pjax:end', NProgress.done);
$(document).pjax('a[data-pjax]', 'body');
</script>
```
上面的代码搭配了nprogress使用，让pjax请求的时候有个进度条的效果。

然后php这边检测是否是pjax请求来确定是否只是输出块而不是输出完整的html文档。

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

```php
<?php
namespace Blog\Controller;
use DI\Annotation\Inject;
use Blog\Model\ArticleRepository;
use DI\Container;
use Doctrine\DBAL\Cache\QueryCacheProfile;
use Symfony\Component\HttpFoundation\Request;
class HomeController extends BaseController
{
    /**
     * @Inject()
     * @var ArticleRepository
     */
    private $repository;
    /**
     * @var Container
     */
    private $container;
    /**
     * Example of an invokable class, i.e. a class that has an __invoke() method.
     *
     * @see http://php.net/manual/en/language.oop5.magic.php#object.invoke
     *
     * @param Request $request
     *
     * @return \Symfony\Component\HttpFoundation\Response
     */
    public function __invoke(Request $request)
    {
        $data['articles'] = $this->repository->getArticles();
        return $this->render('home.twig', $data, $request->headers->get('X-PJAX'));
    }
}
```

## 静态博客中使用pjax

上面的配合php的用法需要服务器上支持php，但是hexo-blog是静态的博客无法在服务端判断是否是pjax请求。

然而这个问题jquery.pjax.js已经解决了，只需要在调用的时候传入`fragment`参数就可以了。

代码如下：
```javascript
$(document).pjax('a[target!="_blank"]', '#pjax-container', {timeout: 8000, fragment: '#pjax-container'});
```
这个代码的意思就是从请求回来的内容里面把`#pjax-container`里面的内容取出来，放到`#pjax-container`里面。

[NexT主题](https://github.com/iissnan/hexo-theme-next) 对应新增文件。

https://github.com/luokuncool/hexo-theme-next/blob/master/layout/_scripts/pjax.swig