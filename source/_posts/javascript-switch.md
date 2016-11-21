---
layout: post
title:  "JS中switch中的比较方式"
date:   2016-11-21 13:48:34 +0800
tags: javascript
---

这里有一段代码: 

```javascript
var a = '1';
  switch(a) {
    case 1:
      console.log(1);
      break;
    default :
      console.log('default');
 }
```

本来期望打印出的是1，结果打印出了default，why?  
原来js的switch在比较的时候使用的是全等, '1' === 1 显然返回了false
    

