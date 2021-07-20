---
title: Echarts Vue 组件
date: 2021-1-5 08:57
tags: 
    - echarts
    - vue
categories: '2021'
---
一个用Vue简单封装的Vue组件，以备不时之需

<!-- more -->

```javascript
npm install echarts -s
```

```html
<!-- template -->
<template>
    <div :style="{height:height,width:width}" :class="className" :id="id" ></div>
</template>
```

```js
/* script */
window._CHARTLIST_ = []  // 罗列出全局所有的echarts实例，for循环调用实例的resize方法实现自适应
import * as echarts from 'echarts';
import lodash from '@/utils/lodash-util'
export default {
    props: {
        id: {
            type: String,
            required: true
        },
        className: {
            type: String,
            default: 'chart'
        },
        width: {
            type: String,
            default: '100%'
        },
        height: {
            type: String,
            default: '100%'
        },
        options: {
            type: Object,
            required: true,
            default: () => ({})
        }
    },
    data() {
        return {
            chart: null,
        }
    },
    methods:{
        /* 初始化 echarts */
        initChart(){
            if(!this.chart) {
                const chartDom = document.querySelector(`#${this.id}`)
                this.chart = echarts.init(chartDom);
                this.chart.setOption(this.options, true);
                window._CHARTLIST_.push(this.chart)
                window.addEventListener('resize', this.resize)
            } else {
                this.chart.setOption(this.options, true);
                this.chart.resize()
            }
        },
        /* echarts 自适应 */
        resize() {
            const { chartObj } = this
            const resizeFunc = function() {
                if(window._CHARTLIST_&&Array.isArray(_CHARTLIST_)) {
                    window._CHARTLIST_.forEach(item => {
                        (item.resize&&item.resize())
                    })
                }
            }
            lodash(resizeFunc, 300)
        }
    },
    mounted: function () {
        this.initChart();
    },
    beforeDestroy() {
        if(this.chart) {
            if(window._CHARTLIST_&&Array.isArray(_CHARTLIST_)) {
                window._CHARTLIST_.forEach((item, index) => {
                    (item.id === this.chart.id&&window._CHARTLIST_.splice(index, 1))
                })
            }
            this.chart = null
            window.removeEventListener('resize', this.resize)
        }
    },
    watch: {
        options: function(n) {
            /* 判断传参是否空对象 */
            if(Object.keys(n).length > 0) {
                this.initChart()
            }
        }
    }
}
```

```js
/* lodash */
/* 自定义 模拟 lodash */
let timeout;
export default (func, wait) => {
    if (timeout) {
        clearTimeout(timeout)
        timeout = null
    }

    timeout = setTimeout(() => {
        func()
        clearTimeout(timeout)
        timeout = null
    }, wait)
}
```