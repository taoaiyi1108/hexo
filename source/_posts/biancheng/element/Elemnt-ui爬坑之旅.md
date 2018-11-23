---
title: Elemnt-ui爬坑之旅
date: 2018-11-23 11:36:52
tags: element
categories: 编程
---
本文记录使用ElementUI开发管理系统时遇到的问题

<!-- more -->

##### form表单验证
- BUG：表单必填项再输入内容后，使用`Backspace`删除回退，再内容清空时出现英文错误提示语
<div>![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/element/pc-element1.png)</div>
- 解决方法：
```html
   safeProductionCode: [
     {
        validator: validateCheckSafeProductionCode,
        trigger: "blur"
     },
     {
        required: true,
        message:'安全生产许可证编号不能为空' 
        <!--必须要有 message 提示信息 就会出现英文错位提示语-->
     }
   ],
```