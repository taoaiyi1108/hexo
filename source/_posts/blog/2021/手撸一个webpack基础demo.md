---
title: 手撸一个webpack基础demo
date: 2021-02-02 01:25:26
tags: webpack
categories: 编程
---
没事了，搞搞，说不定有不一样的收获

<!-- more -->

### Webpack5 基础踩坑记录 [代码.zip](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/webpack/webpack-juejin-learn.zip)

#### dev启动配置

这是`webpack4.x`的写法，依赖有`webpack-cli webpack-dev-server`
```json
"start": "webpack-dev-server --config webpack.config.js"
```

然而使用`webpack5.x`构建项目的时候，就会出现这样的错误, `Cannot find module 'webpack-cli/bin/config-yargs'`, 原因是 webpack-cli与webpack-dev-server版本不兼容；

解决方法：[高手指点](https://www.zihanzy.com/articles/305)

- 降级webpack-cli，安装`webpack-cli 3.*` `npm install webpack-cli@3 -D`
- 最新的webpack-cli已经包含了webpack-dev-server的功能，其实还是要安装`webpack-cli`和`webpack-dev-server`, 不过都是最新的版本，如果在执行 `npm i @webpack-cli/serve -D`, 前边2个以来，会有提示，直接回车就可以了。完成后启动命令有所改变。

```json
"start": "webpack serve --config webpack.config.js"
```


### Webapck-plugin 功能

- `clean-webpack-plugin` 每次执行npm run build 会发现dist文件夹里会残留上次打包的文件，这里我们推荐一个plugin来帮我们在打包输出前清空文件夹clean-webpack-plugin
- `autoprefixer` css前缀
- `mini-css-extract-plugin` 打包后css从js中抽离出来
- `extract-text-webpack-plugin` `webpack4`后官方建议使用`mini-css-extract-plugin`

### Loader使用技巧 [官方文档](https://webpack.docschina.org/concepts/loaders/#inline)

- 内联方式`import Styles from 'style-loader!css-loader?modules!./styles.css';`

### CSS 处理

- `postcss-loader`配合`autoprefixer`添加css前缀，处理兼容性
- `mini-css-extract-plugin` 打包后css从js中抽离出来

### 用babel转义js文件

- `babel-loader` `babel-core`
    - `babel-loader`只会将 ES6/7/8语法转换为ES5语法，但是对新api并不会转换 例如(promise、Generator、Set、Maps、Proxy等),此时我们需要借助`babel-polyfill`来帮助我们转换
    - 注意 `babel-loader`与`babel-core`的版本对应关系
    1. `babel-loader` 8.x 对应`babel-core` 7.x
    2. `babel-loader` 7.x 对应`babel-core` 6.x

### 学习地址
- 这篇教程写的挺好的，不过有些东西还需要自己的实践踩坑，[传送门](https://juejin.cn/post/6844904031240863758)


### 写在最后

- `webpack`真的是难啃啊
- 学好`english`很重要，不然文档看的你怀疑人生

![](https://user-gold-cdn.xitu.io/2019/12/11/16ef406ec8b2c55f?imageslim)