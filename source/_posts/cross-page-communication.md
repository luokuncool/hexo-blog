---
title: 音乐站跨页面添加歌曲实现
tags:
  - HTML
  - 前端
date: 2017-05-26 11:07:32
---


今天突然想听听伍佰的歌，就搜了一个音乐站。

然后来到了虾米音乐，找了找到伍佰的歌点了播放，然后新开了一个页面。

新页面就是播放器页面，然后我还想添加更多的歌曲到播放器，然后我把歌曲列表翻页，选中了点击播放，然后播放器那个页面就自动播放了我选中的歌曲。

好神奇，怎么实现的？

> 我网上搜了搜有说用ajax轮询。

歌曲列表页点击播放的时候发送一个异步请求到服务端。播放器页面轮询是否有新歌曲进来。

但是我查看了网络监控里面点添加歌曲的时候，歌曲列表页并没有网络请求。播放器页面也没有ajax轮询请求。

然后我想到了是不是用了websocket，但是网络ws那一栏也没有信息。

> 后来看到有人说可能是通过cookie来实现的。

歌曲列表在点击播放的时候修改cookie。播放器页定时检测cookie的变更。

然后我分别获取了，点击播放前后cookie值，果然不一样。

那么，来段测试代码吧。

歌曲列表页面代码：
```html
<script type="text/javascript">
window.onload = function() {
  var btn = document.getElementById('playBtn');
  btn.addEventListener('click', function() {
    document.cookie = 'play_list='+encodeURIComponent('1,2,3');
  })
}
</script>
<div id="playBtn">播放选中</div>
```

播放器页面代码：
```html
<script type="text/javascript">
window.onload = function() {
  var m = document.cookie.match(/play_list=(.+?)(?=;|$)/),
      playList = m ? decodeURIComponent(m[1]) : '';
  setInterval(function() {
     var m = document.cookie.match(/play_list=(.+?)(?=;|$)/),
          newList = m ? decodeURIComponent(m[1]) : '';
     if (newList !== playList) {
         //@todo 播放列表变更
     }
  }, 500);
}
</script>
```

实际实现肯定比这个复杂，涉及到歌曲排序，新增等等问题。示例代码就不实现这些了。