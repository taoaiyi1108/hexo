---
title: vue中css样式文字溢出省略
date: 2018-04-23 08:59:28
tags: vue
categories: 编程
---
vue打包时webpack不编译-webkit-box-orient: vertical

<!-- more -->

- 话不多说先看前后效果图
<div align=center>
    ![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/vue/1.png)
</div>
<div align=center>
    ![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/vue/2.png)
</div>

- 图一是本地开发时使用css样式处理文本溢出，样式显示正常，末尾有...
- 图二是使用webpack打包之后，样式出现问题


- 审查元素经过前后对比，发现样式有问题的样式少了-webkit-box-orient: vertical （灰色的不编译）
<div align=center>
    ![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/vue/3.png)
</div>

- 之前也是用vue写的同样的样式本地打包变异后是ok的，和本次不用之处在于前一个项目直接用css，而这次使用的是less，是不是这里的问题

- 经过一番度娘果然有很多人遇到和我一样的问题，[链接](https://segmentfault.com/q/1010000009360389)

- 解决方法各说云词，本人也没有一一去验证，我只采用了下面这个方法，有效，css样式异常问题解决, 但此方法会导致移动端部分手机rem失效，css加载异常

```javascript
optimize-css-assets-webpack-plugin这个插件的问题
注释掉webpack.prod.conf.js中下面的代码
new OptimizeCSSPlugin({
  cssProcessorOptions: config.build.productionSourceMap
    ? { safe: true, map: { inline: false } }
    : { safe: true }
}),
```
- 这中方法测试也有效果 [链接](https://blog.csdn.net/qq_25335529/article/details/80268309)
```css
/*! autoprefixer: off */
-webkit-box-orient: vertical;
/* autoprefixer: on */

.user-info {
    margin: 5px 10px;
    font-size: .8em;
    text-indent: 2em;
    display: -webkit-box;
    -webkit-line-clamp: 4;
    /*! autoprefixer: off */
    -webkit-box-orient: vertical;
    /* autoprefixer: on */
    overflow: hidden;
    text-overflow: ellipsis;
}
```