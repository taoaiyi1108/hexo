---
title: CSS记录点
date: 2020-05-28 14:02:27
tags: css
categories: 编程记录
---
记录开发中不常用，但又很有效果的CSS，或者CSS布局上的技巧

<!-- more -->

> #####  DOM元素添加 disabled 样式，禁止点击事件

```css
	cursor: not-allowed // 鼠标不可点击状态
	pointer-events:none // 鼠标原有事件不能实现
```

<img src="https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/css/css-disabled.jpg" style="width: 250px;" />



> ##### DOM  计算元素宽度时效的问题(虽说是小问题记录下，免得忘记， 也就是bfc布局)

```html
<div class="tabs_line">
    <div class="tabs_container" ref="tabs_container">
        <span :class="['tabs_item', nav_item_active === item.value?'isActive':'']" v-for="item in tabList" :key="item.value" @click="tabsChange(item)">{{item.label}}</span>
    </div>
</div>
```

```css
.tabs_container {
    white-space: nowrap;
}
.tabs_item {
    display: inline-block;
}

修改后的css

.tabs_line {
    overflow: hidden;
}
.tabs_item {
    float: left;
    white-space: nowrap;
}

```

```js
const tabs_line_width = this.$el.clientWidth;
const tabs_container_width = this.$refs.tabs_container.clientWidth;
console.log(tabs_line_width, tabs_container_width)// 850 850    tabs_container_width实际宽度1000  修改css后width为实际值
```

