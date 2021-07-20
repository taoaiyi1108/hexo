---
title: Vue全局事件通信
date: 2021-1-5 09:35
tags: 'vue'
categories: '2021'
---
简易封装Vue全局通信事件
<!-- more -->
Vue 全局通信我们一般采用的是`Vuex`的解决方案，但有的时候，就是一个简单的`$emit`触发机制，使用`Vuex`就不见的那么灵活，

这个时候很多人就会采用`new Vue()` `$on` `$emit`
```js
const e = new Vue();
/* 注入事件 */
e.$on('eventName', function(msg) {
    console.log('我是全局自定义事件传过来的参数msg'+ msg)
})
/* 调用事件 */
e.$emit('eventName', msg)
```

如上既可以父子组件通信，兄弟组件也可以通信，甚至阿婆阿嫂组件也可以，但这样有个问题，`Vue`类太大了，杀鸡焉用牛刀，我们来把他简化一下

```js
/* 事件触发器 */

class EventEmitter {
    constructor() {
        this.subs = Object.create(null) // 对象没有原型属性提高性能
    }

    // 注册事件
    $on(eventType, handler) {
        this.subs[eventType] = this.subs[eventType] || []
        this.subs[eventType].push(handler)
    }
    //触发事件
    $emit(eventType, msg) {
        if (this.subs[eventType]) {
            this.subs[eventType].forEach(handler => {
                handler(msg)
            })
        }
    }
}

const em = new EventEmitter()

export default em
```

使用
```js
import EventEmitter from './EventEmitter.js'

EventEmitter.$on('refresh', function(msg) {
    conosle.log(msg)
})

EventEmitter.$emit('refresh', msg)
```