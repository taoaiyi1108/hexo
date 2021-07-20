---
title: DingDing小程序开发总结
date: 2021-1-25 23:25
tags: '小程序'
categories: '2021'
---
书到用时方恨少，事非经过不知难

<!-- more -->

前几天帮别人开发了一个Ding Ding小程序，说实在的也是第一次开发这类业务，临时抱佛脚，赶鸭子上架，虽然很痛苦，可收获也是满满的

### 业务背景

- 主要是用于智慧工地，从上至下对于项目问题和工程进度的把控及问题反馈处理。
    - 项目新建
    - 项目审批
    - 项目流程流转
    - 项目人员配置
    - 项目问题提交
    - 项目问题处理
    - 项目位置导航
    - ...

- 公司日常办公基于钉钉，客户提出的要求就是做一个钉钉上的程序， 我这边选择的是钉钉企业内部应用

### 开车指南

1. 钉钉的小程序和微信的基本差不多，但生态还是差的有点多，所以说很多业务实现起来没有现成的组件使用，需要自己造， 当然了你可以借鉴官方提供的例子，里面也是有很多好用的DEMO

### 登录权限

```
    用户登录 ---> 本地缓存用户信息 ---> 不同权限用户tabbar不同 ---> 跳转页面不同

```
![权限1用户](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/xiaochengxu/dd-1.png)
![权限2用户， 虽然和权限1的tabBar名称一样，但是跳转的页面不一样](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/xiaochengxu/dd-3.png)
![权限3用户](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/xiaochengxu/dd-2.png)

- 刚开始我想到的是微信小程序不是有自定义tabBar么，就采用这套方案

```
调用dd.addTabBarItem添加tabBar页面。
调用dd.removeTabBarItem移除tabBar页面。

注意 使用该接口请注意：
    addTabBarItem 调用时，要保证当前小程序展示的是TabBar上的页面；否则调用会报错，错误码 11。
    PS: 我是有原则的人，不是以想用就可以用的
```

好吧你有原则， 我可以让你变得无原则，按照官方的API修改后，可以实现不同用户权限展示不同的tabBar，但是问题又来了

问题：用户退出登录，不关闭小程序，直接更换账号登录，直接给我把2个不同权限用户的tabBar全展示出来

好家伙还好最多5个，不然你还给起飞了不成，排查后发现是缓存造成的，只有退出小程序缓存才可以清除，这咋搞

那这条路子是走不通了，只能自己通过模板的形式自定义了，话不多说直接开干

```html 
<template name="tabbar-1">
    <view class="tabbar">
        <view class="tabbar-item" a:for="{{tabbarlist}}" data-url="{{item.pagePath}}" onTap="tabbarTo">
            <image class="tabbar-item_icon" mode="scaleToFill" src="{{active == item.index ? item.activeIcon : item.icon}}"/>
            <text class="tabbar-item_text" style="color: {{active == item.index?'#3780F0':'#737373'}}">{{item.name}}</text>
        </view>
    </view>
</template>
```

```js
export default {
    tabbarTo(e) {
        const { url } = e.target.dataset
        dd.reLaunch({ url })
    }
    /* 
        reLaunch  调用dd.reLaunch关闭当前所有页面，跳转到应用内的某个指定页面。  

        
    */
}
```

```html
<!-- 使用模板 -->
<import src="/template/tabbar/index.axml"/>
<template a:if="{{location == '1'}}" is="tabbar-1" data="{{active: 1, tabbarlist: tabbarlist}}"/>
```


#### 总结：
1. 自定义tabbar模板，使用dd.reLaunch进行切换
2. 不使用 dd.redirectTo和dd.navigateTo的原因是因为页面跳转后都会返回箭头，展示逻辑和原生的tabBar不太一样
3. 使用dd.reLaunch进行切换就会失去缓存，页面每次都是查询加载，性能上有点弱


### 父组件调用子组件的方法, 子组件传值给父组件

微信小程序中是有`triggerEvent`， 钉钉小程序中有所不同， 采用的是`props`的方式来实现


#### 子组件

```html
<view class="user-card-item_left" data-value="{{item.id}}" onTap="radioChange"></view>
```
```js
props: {
    onChange(value) {}
}
methods: {
    radioChange(e) {
        const { value } = e.target.dataset
        this.props.onChange(value)
    }
}
```

#### 父组件

```html
<Children onChange="onChange" />
<!-- 注意自定义方法必须以on开头，不然不会生效 -->
```
```js
methods: {
    onChange(value) {
        /* value 子组件传递过来的数据 */
        this.setData({ userIds: value })
    }
}
```