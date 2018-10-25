---
title: vue中使用eachars
date: 2018-03-20 16:28:46
tags:
	- vue
	- echarts
categories: 编程
---
Vue中使用echarts

<!-- more -->

##### echarts[官网](http://echarts.baidu.com/)

##### 安装

	npm install echarts -S

#### 全局引入（main.js）
	import echarts from 'echarts'
	Vue.prototype.$echarts = echarts 

#### 使用
```html
<template>
    <div id='myChart'></div>
</template>
```

```javascript
<script type="text/ecmascript-6">
    require('echarts/map/js/world')//引入地图
    require('echarts/theme/shine') // 引入主题
    methods(){
        drawLine(){
        let myChart = this.$echarts.init(document.getElementById('myChart'),'主题')
            let option = {} //参数配置
            myChart.setOption(option); //初始化
            myChart.resize()//自适应大小
        }
    }
</script>
```
```css
<style lang="scss" scoped>
  .#myChart {
      width: 800px;
      height: 600px;
    }
</style>
```