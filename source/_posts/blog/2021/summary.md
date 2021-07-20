---
title: 零散的知识点总结
date: 2021-1-21 13:46
tags: '知识点'
categories: '2021'
---
好记性不如一个烂笔头，有备无患吧

<!-- more -->

#### 1. Vue

- `scope`中覆盖全局css样式
```html
<style lang="css" scoped>
    /deep/.demo {
        font-size: 14px;
    }
</style>
```

#### 2. Nuxtjs
- `scope`中覆盖全局css样式
```html
<style lang="css" scoped>
    ::v-deep .demo {
        font-size: 14px;
    }
</style>
```