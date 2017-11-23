---
layout: post
title:  "video在微信浏览器（webview）下不能自动播放解决方法"
date:   2017-04-25 15:28:00 +0800
description: "最近遇到一个需求，在微信浏览器中视频要求自动播放，解决方法在此作个备忘"
author: 武明礼
categories: front article
---

在微信的webview下，视频做了好多限制，例如无法通过autoload、Element.play()等方法控制视频的自动播放，隐藏视频播放控制进度条等。

话不多说，上代码（Talk is cheap, show me the code）：

```html
<div id="ghComments">
  <img class="video-img" src="http://img.58cdn.com.cn/zhuanzhuan/zzactivity/magic/phone2.jpg" alt="">
</div>
```

相关JS代码（偷懒，用的jq）

```js
$(function () {
	$('.video-img').one('click', function () {
		var vd = document.createElement('video');
		vd.src = 'http://200051983.vod.myqcloud.com/200051983_09cb0d60147f11e78757737a057ef66b.f20.mp4';
		vd.width = 320;
		vd.height = 240;
		vd.autoplay = true;
		vd.preload = 'auto';
		vd.poster = 'http://img.58cdn.com.cn/zhuanzhuan/zzactivity/magic/phone2.jpg';
		vd.id = 'video1';
		vd.setAttribute('webkit-playsinline', 'webkit-playsinline');
		vd.setAttribute('playsinline', 'playsinline');
		$(this).parent().html(vd);
		vd.play();
		document.addEventListener("WeixinJSBridgeReady", function () {
			$('#video1')[0].play();
		}, false);
	});
});
```

### 说明

微信中，需要添加微信的jssdk，然后给document绑定WeixinJSBridgeReady事件，否则无法（自动）播放。所以以上代码需要放在微信js引用后方可生效：

```html
<script src="//res.wx.qq.com/open/js/jweixin-1.0.0.js"></script>
```
