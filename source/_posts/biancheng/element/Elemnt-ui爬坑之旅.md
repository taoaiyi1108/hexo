---
title: Elemnt-ui爬坑之旅
date: 2018-11-23 11:36:52
tags: element
categories: 编程
---
本文记录使用ElementUI开发管理系统时遇到的问题和功能实现技巧

<!-- more -->

##### 推荐牛人对ElementUI的源码解析   [地址](https://www.cnblogs.com/fangnianqin/category/1357367.html)

##### form表单验证
- BUG：表单必填项再输入内容后，使用`Backspace`删除回退，再内容清空时出现英文错误提示语
<div>![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/element/pc-element1.png)</div>
- 解决方法：

```javascript
safeProductionCode: [
     {
        validator: validateCheckSafeProductionCode,
        trigger: "blur"
     },
     {
        required: true,
        message:'安全生产许可证编号不能为空' 
        /*必须要有 message 提示信息 就会出现英文错位提示语*/
     }
    ]
```

##### table 点击行获取当前行的index
- 直接上代码
```html
    <el-table
        :data="list"
        :row-class-name="tableRowClassName"
        border
        @row-click="rowClick"
    </el-table>
```
```javascript
    export default  {
        methods:{
            /*把每一行的索引放进row*/
            /*注意形参是一个对象哦*/
            tableRowClassName({row, index}){
                row.index = index;
            },
            /*在获取row的index*/
            rowClick(row, event, column){
                const index = row.index;
            }
        }
    } 
```



##### element-ui dailog中el-cascader数据不刷新渲染

- 部门地址中的省市县三联动详情展示

  <img src="https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/element/el-cascader-1.png" alt="1" style="zoom: 80%;" />

  <img src="https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/element/el-cascader-2.png" style="zoom:50%;" />

  

<img src="https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/element/el-cascader-3.png" style="zoom:50%;" />

在第一次三级全部被选择后，其他的部门地址就不会被渲染，检查发现el-cascader中的prop属性方法不会被出发，类似于缓存了，好，那么就不让他缓存，

```js
<el-cascader v-if="dialog-show" v-model="detpinfoForm.addressList"  placeholder="请选择省/市/区" :props="addressOptions" :disabled="isDisable" filterable clearable></el-cascader>

// dialog-show : 在el-dialog每次弹出时都初始化一个新的el-cascader， 隐藏时销毁el-cascader
```

##### 解决ElementUI导航栏中的vue-router在3.0版本以上重复点菜单报错问题

```js 
const originalPush = VueRouter.prototype.push
VueRouter.prototype.push = function push(location) {
  return originalPush.call(this, location).catch(err => err) // 重写push不让打印错误日志
}
```

![](https://img-blog.csdnimg.cn/20200602152615772.png#pic_center)





