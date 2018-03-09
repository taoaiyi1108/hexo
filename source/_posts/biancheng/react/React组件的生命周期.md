---
title: React组件的生命周期总结
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

##### 代码书写小贴士

```
      a.命名方法
	      i. 组件的私有方法都用 _ 开头，
	      ii. 所有事件监听的方法都用 handle 开头。
	      iii. 把事件监听方法传给组件的时候，属性名用 on 开头
      d. 组件的内容编写顺序如下
          i. static 开头的类属性，如 defaultProps、propTypes。
          ii. 构造函数，constructor。
          iii. getter/setter（还不了解的同学可以暂时忽略）。
          iv. 组件生命周期。
          v. _ 开头的私有方法。
          vi. 事件监听方法，handle*。
          vii. render*开头的方法，有时候 render() 方法里面的内容会分开到不同函数里面进行，这些函数都以 render* 开头。
          viii. render() 方法

```