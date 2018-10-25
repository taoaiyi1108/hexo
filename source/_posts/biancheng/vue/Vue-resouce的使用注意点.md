---
title: Vue-resouce的使用注意点
date: 2018-09-21 11:59:03
tags: 
    - vue 
    - vueresouce
categories:
---
在vue-cli中使用vue-resource的两种方式：

<!-- more -->


- 在component中使用：
```JavaScript
methods：{
    getdata(){
        this.$http.jsonp(url).then(response =>{
            // 这里的this指的就是vue
        })
    }
}
```
- 在入口文件main.js或者其他的api.js文件中使用：

```javascript
    // main.js 中我们已经引入和vue.use(Resource)了
    Vue.http.jsonp(url).then(response =>{
        // 这里就不能再用this了
    })
```
