---
title: 缩略图自适应大小且居中
date: 2017-06-19 16:24:03
tags: css
---

之前碰到一个关于样式上的问题。
![](https://ooo.0o0.ooo/2017/06/19/59478ce7ddaa2.png)

在未知图片大小的情况下，图片的大小要随着移动端的变化而变化。
<!-- more -->

```
<li>
	<div class="image">
      <div class="image-inner">
        <img src="https://ooo.0o0.ooo/2017/06/19/59478f26d55b5.png">
      </div>
    </div>
</li>
```

样式 

```
li .image{
	padding-top: 100%;
	position: relative;
	height: auto;
	text-align: center;
}
li .image .image-inner{
	width: 100%;
	height: 100%;
	position: absolute;
	top: 0px;
	left: 0px;
	white-space: nowrap;
}
li .image .image-inner:after{
	content: '';
	width: auto;
	height: 100%;
	vertical-align: middle;
	display: inline-block;
}
li .image .image-inner img{
	max-width: 100%;
	max-height: 100%;
	vertical-align: middle;
}
```

原理是用after里的inline-block起到一个参考的作用，取得两个的中线，从而完成垂直居中

![](https://i.stack.imgur.com/XIFPv.png)

还有一个奇淫技巧吧……

```
img {  
    max-height: 100%;  
    max-width: 100%; 
    width: auto;
    height: auto;
    position: absolute;  
    top: 0;  
    bottom: 0;  
    left: 0;  
    right: 0;  
    margin: auto;
}
```

[https://stackoverflow.com/questions/7273338/how-to-vertically-align-an-image-inside-div](https://stackoverflow.com/questions/7273338/how-to-vertically-align-an-image-inside-div)



