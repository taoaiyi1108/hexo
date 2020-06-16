---
title: Web API 不知道的就记录
date: 2020-06-08 10:00:24
tags: Web API
categories: 编程
---
Web API 不知道的就记录

<!-- more -->

> ``**Document.visibilityState**` （只读属性）`

```js
var string = document.visibilityState

document.addEventListener("visibilitychange", function() {
  console.log( document.visibilityState );
  // visibility or hidden 类似于小程序的一个页面的show和hide
});

```



> `Element.getBoundingClientRect() 方法返回元素的大小及其相对于视口的位置`

```js
document.getElementById('语法').getBoundingClientRect()
```

![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/webapi/webapi-getBoundingClientReact.png)

> `window.opener`判断页面是否通过`window.open`打开的