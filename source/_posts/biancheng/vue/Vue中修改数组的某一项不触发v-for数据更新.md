---
title: Vue中修改数组的某一项不触发v-for数据更新
date: 2020-06-01 14:38:15
tags: vue
categories: 编程
---
<!-- more -->

[官网Vue-set](https://cn.vuejs.org/v2/api/#Vue-set)

```html
<ul>
    <li v-for="(item, index) in list" :key="index" @click="changeItemName(index)" >{{item.name}}</li>
</ul>
```

```js
data() {
    return {
        list:[
            {name: '张三'},
            {name: '李四'}
        ]
    }
}

methods: {
    changeItemName(index) {
        this.list[index] = {name: '王五'} // 此时你会发现 页面列表并未刷新修改数据
        
        // 正确写法是
        this.$set(this.list, index, {name: '王五'})
    }
}
```

