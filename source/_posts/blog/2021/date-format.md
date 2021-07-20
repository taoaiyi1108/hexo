---
title: Date format 时间格式化
date: 2021-1-5 14:09
tags: 
    - date 
    - format
categories: '2021'
---
自己封装一些时间格式化脚本，以备不时之需

<!-- more -->

时间格式化
```js
export const dateformat = function (format, fmt) {
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
    var date = format ? new Date(format) : new Date();
    return formatDate(date, fmt || "yyyy-MM-dd");
}

/* 用法 */
dateformat(new Date(), "yyyy年MM月dd日 hh:mm:ss") // 2020年12月31号 12:59:59
```

星期格式化
```js
export const weekformat = function (formart) {
    const weeklist = ['日', '一', '二', '三', '四', '五', '六']
    const week = formart ? new Date().getDay() : new Date().getDay()
    return `星期${weeklist[week]}`
}

/* 用法 */
dateformat(new Date()) //星期四
```