---
title: mint-ui-Lazyload的用法
date: 2017-11-29 18:00:43
tags:
	- vue
	- mint-ui
categories: 编程

---

### [官方文档](http://mint-ui.github.io/docs/#/zh-cn2/lazyload)
- 在官方使用手册上，结合本人的爬坑之旅加以补充、
- mint-ui的lazyload其实就是把Lazyload的源码搬过来，所以说具体的直接去看Lazyload的源码就可以了

### 引入
-全局引入和局部引入
```javascript
	//全局引入(main.js)
	improt mintui from 'mint-ui';
	Vue.use(mintui);
	//局部引入(当前模块)
	import Vue from 'vue'
	import { Lazyload } from 'mint-ui';
	Vue.use(Lazyload);
	//为了便于设置 推荐局引入
	//不管是去全局还是局部引入 都要记得在mian.js 中引入 mint-ui的css 之前好多次只有引入了模块忘记引入css，导致效果出不来，还不知道哪里错误了
	import 'mint-ui/lib/style.min.css';
```
```html
	<ul>
	  <li v-for="item in list">
	    <img v-lazy="item">
	  </li>
	</ul>
```
```css
	img[lazy=loading] {
	  width: 40px;
	  height: 300px;
	  margin: auto;
	/*必要时可以加背景色*/
	}
```

### 设置加载图片和加载失败的图片
```html
 <img v-lazy="图片路径" :onerror="默认失败路径">
```
```javascript
  import Vue from 'vue'
  import { Lazyload } from 'mint-ui';
  Vue.use(Lazyload,{
    preLoad: 1.3, //加载时长
    loading: 'static/images/loading.gif', //加载中图片路径
    error: 'static/images/error.png',//加载失败图片路径
  });
```