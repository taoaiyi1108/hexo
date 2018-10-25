---
title: vue中的数据过滤
date: 2018-08-27 09:45:40
tags: vue
categories: 编程
---
vue中过滤器的几种用法

<!-- more -->

##### 正常的filter用法
- 需求：请求回来的数据中@@要替换成DOM换行元素`<br/>`
```
请求回来的数据格式： A:你好？@@B:嗯，你也好！

展示的格式：
    A:你好？
    B:嗯，你也好！
```
- html
```html
    <!-- vue -->
    <template>
        <p class="data">{{Data | dataFilter}}</p>
    </template>
```
- javascript
```javascript
    data(){
        return {
            Data:''
        }
    },
    methods:{
        getdata(){
            Ajax.jsonp(url).then( response =>{
                this.Data = response.data;
            })
        }
    },
    filters:{
        dataFilter:function(text){
             return text.replace(/@@/g, "<br/> ");
        }
    }
```
- 页面展示结果
```html
A:你好？<br/>B:嗯，你也好！
<!-- 好像不对啊，br 怎么直接变成文本输出了，没有转换成一个HTML元素 -->
<!-- 这种方法不行那我们就换下种方法 -->
```

##### v-html 中使用filter的功能，[参考链接](https://blog.csdn.net/qq_34971175/article/details/79719058)

- html
```html
    <!-- vue -->
    <!-- v-html 在此使用filters的写法和第一种就不一样了不要写错哦 -->

    <!-- 错误写法 Vue2.0 不再支持在 v-html 中使用过滤器 -->
    <template>
        <p class="data" v-html="Data | dataFilter"></p>
    </template>

    <!-- 正确写法  $options.filters -->
    <template>
        <p class="data" v-html="$options.filters.dataFilter(Data)"></p>
    </template>
```
- javascript
```javascript
    // js 和上面的写法一样 在此就不多 赘述
```
- 页面展示结果
```html
A:你好？
B:嗯，你也好！
<!-- 这下写对了么 -->
```

###### 最后分享一个时间过滤器，备用哦
```javascript
    // 时间过滤器
    timefilter: function (format) {
        function formatDate(date, fmt) {
            if (/(y+)/.test(fmt)) {
                fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length));
            }
            let o = {
                'M+': date.getMonth() + 1,
                'd+': date.getDate(),
                'h+': date.getHours(),
                'm+': date.getMinutes(),
                's+': date.getSeconds()
            };
            for (let k in o) {
                if (new RegExp(`(${k})`).test(fmt)) {
                    let str = o[k] + '';
                    fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? str : padLeftZero(str));
                }
            }
            return fmt;
        };
        function padLeftZero(str) {
            return ('00' + str).substr(str.length);
        }
        var date = new Date(format);
        return formatDate(date, "yyyy-MM-dd");
    },
```