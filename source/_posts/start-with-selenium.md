---
title: Selenium初学者问题记录
tags:
  - Java
  - Selenium
date: 2017-04-18 11:41:28
---


## 前言

前段时间公司一个需求遇到一个需要把`A系统`里面的信息提交到`B系统`。`A系统`是公司内部的，`B系统`不是。

并且B系统并不对外提供对应的接口，需要使用硬件证书+`极验验证码`登录，只支持IE。我无法在不启动浏览器的情况下绕过登录提交信息。

所以采用了Selenium来模拟人为登录。因为第一次使用Selenium这里记录下，对于初学者会遇到的一些基础问题。

## 问题列表

> 下面的`driver`是`org.openqa.selenium.ie.InternetExplorerDriver`的实例。

### Selenium 无法启动IE浏览器

在使用selenium启动IE浏览器的时候，也许会出现类似以下错误。
```
Exception in thread "main" java.lang.ExceptionInInitializerError
Caused by: org.openqa.selenium.SessionNotCreatedException: Unable to create new remote session. desired capabilities = Capabilities [{ensureCleanSession=true, browserName=internet explorer, version=, platform=WINDOWS
}], required capabilities = Capabilities [{}]
Build info: version: 'unknown', revision: 'unknown', time: 'unknown'
```

这个只是IE浏览器没有设置好。只需要在internet选项里面安全那一栏把所有的启用保护模式复选框去掉。并设置好安全级别分别为`中-高`,`高`,`高`,`高`。在重新运行selenium程序就可以了。

设置如下图：

![](/images/20170418103755.png)

### 选择元素

#### 通过id选择
```
driver.findElement(By.id("eleId"))
```
#### 通过css选择器选择
```
driver.findElement(By.id("#eleId"))
```
#### 通过内容选择
如下代码能够选中第一个包含有`下载`字样的`a`元素。
```
driver.findElement(By.xpath("//a[contains(text(),'下载')]"));
```
这个其实是通过`xpath`来定位的元素。`xpath`能够解决所有元素的定位问题。
当有些元素没有`class`、`id`、`name`或者通过css选择器无法定位的时候，使用xpath来定位是个非常好的选择。

### 选中下拉菜单

```
Select select = new Select(driver.findElement(By.id("eleId")));
select.selectByValue("value");
```

### 往文本框输入值

```
driver.findElement(By.id("eleId")).sendKeys("输入值");
```

### 文件上传
文件上传和文本框输入值一样都是使用`sendKeys`方法

```
driver.findElement(By.id("eleId")).sendKeys("文件路径");
```

### 等待元素加载
在操作元素的时候会遇到元素还没有加载出来的情况，所以需要等到元素加载成功才能对其操作。

```
By by = By.id("eleId");
WebDriverWait wait = new WebDriverWait(driver, timeout);
wait.until(new ExpectedCondition<Boolean>() {
    public Boolean apply(WebDriver webDriver) {
        System.out.println("等待元素加载" + by.toString());
        return webDriver.findElements(by).size() > 0;
    }
});
```

### 等待`alert`弹出窗并关闭

有时候点击完按钮后会弹出`alert`，需要等待它弹出来再关闭它。
```
WebDriverWait wait = new WebDriverWait(driver, 30);
wait.until(ExpectedConditions.alertIsPresent());
driver.switchTo().alert().accept();
```

### 切换到iframe

元素有时候位于`iframe`里面，需要把操作焦点切换到里面才能操作里面的元素。
```
driver.switchTo().frame("iframeId");
```
操作完以后再切换回父框架。
```
driver.switchTo().parentFrame();
```

### 拖放元素
```
WebElement slider = driver.findElement(By.cssSelector(".gt_slider_knob"));
Actions actions = new Actions(driver);
actions.clickAndHold(slider).perform();
Thread.sleep(1000);
for (int i = 0; i < left; i = i + 7 + (int) (Math.random() * 4)) {
    actions.moveToElement(driver.findElement(By.cssSelector(".gt_slider")), i + 20, 0).perform();
}
Thread.sleep(1000);
actions.release(slider).perform();
```

### Selenium截图黑屏

需求中需要把`B系统`系统里面的一个页面截图回传到`A系统`，但是在windows未登录的情况下selenium截图黑屏。

因为这台运行Selenium的计算机是通过远程的方式登录的，大多数情况下都是出于未登录状态。

然后我想到通过java发送http请求来获取html后将html转成图片。但是这个请求的地址是需要登录的。

为了解决这个问题我从Selenium这边获取了登录后的cookie，然后通过http请求将这个cookie发给`B系统`再把响应的html，通过`wkhtmltoimage`转成图片。


## 说两句

第一次使用Selenium也是第一次用Java做东西。说的不对的希望看到的朋友能指正出来。

记录这些为了以后自己翻阅也希望能够帮助到其他第一次使用的朋友。