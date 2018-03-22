---
title: vue中反向代理解决跨域问题
date: 2018-03-22 10:12:15
tags:
    - vue
    - 跨域
categories: 编程
---
<h4>Vue中使用反向代理解决DEV时的跨域问题</h4>

<!-- more -->

##### Vue中baseAPI的配置

- 完整域名(http://127.0.0.1:8088/rockymobi-permission)

- config文件夹下
    - index.js(配置反向代理)
    ```javascript
    module.exports = {
     dev: {
         ...
        //反向代理配置地方
        proxyTable: {
            '/api': {
                target: 'http://127.0.0.1:8088',//需要代理的域名
                changeOrigin: true,// true代表可以跨域
                pathRewrite: {
                '^/api': ''
                }
            }
        ...
    ```
    - dev.env.js(开发环境)
    ```javascript
    module.exports = merge(prodEnv, {
        NODE_ENV: '"development"',
        BASE_API: '"/api/rockymobi-permission"',//必须添加api字段,api表示的代理的之后的域名(也不一定是api，只要保持和代理那边的一样就可以了)
    })
    ```
    - prod.env.js(线上)
    ```javascript
    module.exports = {
        NODE_ENV: '"production"',
        BASE_API: '"http://127.0.0.1:8088/rockymobi-permission"'//一般上线后都在页面都在Java的目录下，不存在跨域
    }
    ```