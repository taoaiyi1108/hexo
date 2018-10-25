---
title: vue-awesome-swiper的使用注意点
date: 2018-09-29 16:07:03
tags:
    - vue 
    - vue-awesome-swiper
categories: 编程
---
vueswiper插件vue-swesome-swiper配坑记录

<!-- more -->

- 最近在做项目的时候使用`npm install vue-swesome-swiper --save` 安装完vue的swiper的插件后，在配置options是发现无限滚动loop和autoplay在设置后不起作用，这是为何？

- 主要的问题是`vue-awesome-swiper`的版本更新了，细心的同学在vue的配置文件`package.json`发现之前的版本是2.6.1,而现在到了3.1.0了；就还好swiper3和swiper4的api有所变化是一样的。

- autoplay 不自动播放？
```javascript
// @2.6.1 配置
swiperOptions :{
    autoplay:1000,
    autoplayDisableOnInteraction:false,
    loop:true
}
//@3.1.0
swiperOptions :{
    autoplay:{
        delay:1000,
         autoplayDisableOnInteraction:false,
    },
    loop:true
}
```
- loop 不循环播放？
```javascript
// 细心的同学是不是已经发现上边的代码设置了loop:true了，而且并没有因为打本不同写法不同,那问题在哪里呢
// 问题其实就在swiper组件的渲染前后的时间上；
// 一般我们的数据都是异步获取的，swiper组件在渲染时候因为没有数据而导致问题出现，为了保证数据全部获取到了再去渲染swiper 我们可以这么做
// 在vue的mounted钩子中获取数据
// 再者使用v-if来判断的数据完全加载后再去渲染swiper组件
```
```html
<swiper>
    <swiper-slider v-for="(v,i) in data" :key="i" v-if="data.length">
        <img :src="v.url" />
    </swiper-slider>
</swiper>
```

- 小弟不才，没看清楚的移步这里[观摩大神](https://www.cnblogs.com/stephentian/p/8344258.html)