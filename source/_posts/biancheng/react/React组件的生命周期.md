---
title: React组件的生命周期
date: 2018-03-08 15:45:33
tags: React
categories: 编程
---
<h4>明白React组件的生命周期就是这么简单</h4>

<!-- more -->

##### React 组件生命周期 

```

  a. 挂在阶段
      i. construct() 组件自身的状态的初始化工作
      ii. componentWillMount() 组件挂载开始之前，也就是在组件调用 render 方法之前调用：Ajax 数据的拉取操作、一些定时器的启动，清理，数据的清理
      iii. render() 
      iv. DOM插入页面
      v. componentDidMount() 组件挂载完成以后，也就是 DOM 元素已经插入页面后调用：DOM的动画
  b. 更新阶段
      i. shouldComponentUpdate(nextProps, nextState)可以通过这个方法控制组件是否重新渲染。如果返回 false 组件就不会重新渲染
      ii. componentWillReceiveProps(nextProps)：组件从父组件接收到新的 props 之前调用
      iii. componentWillUpdate()：组件开始重新渲染之前调用。
      iv. componentDidUpdate()：组件重新渲染并且把更改变更到真实的 DOM 以后调用。
  c. 即将从页面中删除
      i. componentWillUnmount() 组件对应的 DOM 元素从页面中删除之前调用。
      ii. 从页面中删除 


```