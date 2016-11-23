---
layout: post
title:  "避免重复Ajax提交"
date:   2016-11-21 00:26:34 +0800
tags: JavaScript
---

之前在做网站的时候，采用如下代码进行ajax提交，在访客点击的时候请求如果没有及时得到服务器的响应，会尝试反复点击造成重复提交

```javascript
$(function(){
  $('selector').bind('click', function(){
    $.ajax({
      type : 'POST',
      url : 'requestURL',
      dataType : 'json',
      success : function(res) {
    
      }
    });
  })
});
```

为了避免重复提交，将代码改成如下  

```javascript
$(function(){
  $('selector').bind('eventType', function(){
    var thisFunc = arguments.callee, 
    self = $(this);
    self.unbind('click', thisFunc); //一次请求解绑事件监听函数
    $.ajax({
      type : 'POST',
      url : 'requestURL',
      dataType : 'json',
      success : function(res) {
            self.bind('click',thisFunc); //请求完成再次绑定事件监听函数
      }
    });
  })
});
```