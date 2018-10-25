---
title: 前端JS实现复制内容
date: 2018-09-29 17:06:36
tags: js知识点
categories: 编程
---

点击按钮实现复制功能

<!-- more -->

```html
    <div>
        <input type="text" value='被复制的内容' id="copy"/>
    </div>
```
```javascript
    var e = document.getElementById("copy");
    e.select();
    document.execCommand("Copy");
    alert("内容已经复制成功，ctr+v 就可以粘贴了")
```