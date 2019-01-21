---
title: Vue兄弟组件之间通信传值
date: 2019-01-21 16:23:50
tags: vue
categories: 编程
---
Vue兄弟组件之间通信传值

<!-- more -->

- Vue中提供的状态管理工具Vuex是可以实现Vue中兄弟组件之间的通信，但今天我们来学习另外一种简单的方法，借助Vue自身的api接口实现

- utils.js
```javascript
    import Vue from "vue";
    const event = new Vue();
    export default event;
```

- Vue组件A 发出
```html
    <button @click="issue"></button>
```
```javascript
    import event from "utils.js"
    methods:{
        issue(){
            event.$emit("VueEvent",eventContent)
        }
    }
```

- Vue组件B 接收
```javascript
    import event from "utils.js"
    methods(){
        receive(){
            event.$on("VueEvent",(eventContent) =>{
                eventContent.log
            })
        }
    },
    mounted(){
        this.receive();
    }
    
```