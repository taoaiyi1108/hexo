---
title: 父页面触发iframe中click事件
date: 2018-03-22 15:48:59
tags: iframe
categories: 编程
---
<h4>父页面触发iframe中click事件总结</h4>

<!-- more -->
##### 页面布局
- 父页面
```html
<div class="box">
    <button class="btn">父页面按钮</button>
    <iframe src="./index2.html" id="test"></iframe>
</div>
```
```javascript
$(function () {
    $(".btn").on("click", function () {
        var frame = $("#test").prop("contentWindow").document;
        $(frame).find("#rec").trigger("click")
    })
})
```
- iframe页面
```html
<div class="box">
    <button id="rec">iframe按钮</button>
</div>
```
```javascript
$(function () {
    $(function() {
        $("#rec").on("click", function() {
            alert('父组件触发的我')
        })
    }); 
})
```
- 注意事项
    - 页面必须用localhost（服务）形式打开，file形式打开不行
    - 同域名下可以，不同于域名时会出现跨域