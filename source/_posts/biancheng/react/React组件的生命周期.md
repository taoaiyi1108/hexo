---
title: React要点知识总结
date: 2018-03-08 15:45:33
tags: React
categories: 编程
---
<h4>明白React组件的生命周期就是这么简单</h4>

<!-- more -->

##### React  要点知识总结:

```javascript
- 1.class 创建 组件时 组件名必须首字母大写;
- 2.React中的on*的事件监听是能用在普通的HTML标签上，而不能用在组件标签上，而且这些事件的名称都采用驼峰命名;
- 3.React中的on*的事件监听是能用在普通的HTML标签上，而不能用在组件标签上，而且这些事件的名称都采用驼峰命名;
- 4.class ChildClass extends ParentClass {...} 前者继承了后者的方法;
- 5.改变组件状态的时候不能通过this.sate = xxx 这种方式来修改，一定要使用React提供的setState方法，它接受一个对象或者函数作为参数;
- 6.调用setState的时候，React.js并不会马上修改state,而是把这个对象放到一个更新队列里面，稍后才会在队列中把新的状态提取出来合并到state当中，然后在触发组件更新。所以如果在setState之后使用新的state来做后续的运算就做不到了;
- 7.使用 React.js 的时候，并不需要担心多次进行 setState 会带来性能问题;
- 8.每个组件都可以接受一个 props 参数，它是一个对象，包含了所有你对这个组件的配置;
- 9.props 传值，在使用一个组件的时候，可以把参数放在标签的属性当中，所有的属性都会作为 props 对象的键值，组件内部就可以通过 this.props 来访问到这些配置参数;
- 10.一个组件的行为、显示形态都可以用 props 来控制;
- 11.defaultProps 对 props 中各个属性的默认配置;
- 12.props不可变，一旦传入就不能改变;
- 13.子组件向父组件传值：父组件 CommentApp 只需要通过 props 给子组件 CommentInput 传入一个回调函数。当用户点击发布按钮的时候，CommentInput 调用 props 中的回调函数并且将 state 传入该函数即可。
- 14.React 组件生命周期
    - a.挂在阶段;
        - i.construct() //组件自身的状态的初始化工作
        - ii.componentWillMount() //组件挂载开始之前，也就是在组件调用 render 方法之前调用：Ajax 数据的拉取操作、一些定时器的启动，清理，数据的清理
        - iii.render() 
        - iv.DOM插入页面
        - v.componentDidMount() //组件挂载完成以后，也就是 DOM 元素已经插入页面后调用：DOM的动画
    - b.更新阶段        - i.shouldComponentUpdate(nextProps, nextState) //可以通过这个方法控制组件是否重新渲染。如果返回 false 组件就不会重新渲染
        - ii.componentWillReceiveProps(nextProps) //组件从父组件接收到新的 props 之前调用
        - iii.componentWillUpdate() //组件开始重新渲染之前调用。
        - iv.componentDidUpdate() //组件重新渲染并且把更改变更到真实的 DOM 以后调用。
    - c.即将从页面中删除
        - i.componentWillUnmount() //组件对应的 DOM 元素从页面中删除之前调用。
        - ii.从页面中删除 
- 15.能不用ref就不用ref ，DOM，组件都可以添加ref;
- 16.this.props.children;
- 17.dangerouslySetInnerHTML传入一个对象，这个对象的 __html 的属性值就相当于innerHTML
- 18.style  style={{color:'red',fontSize:'12px'}} 驼峰命名法
- 19.isRequired  关键字来强制组件某个参数必须传入
- 20.代码书写小贴士：
    - a.组件的私有方法都用 _ 开头;
    - b.所有事件监听的方法都用 handle 开头。
    - c.把事件监听方法传给组件的时候，属性名用 on 开头
    - d.组件的内容编写顺序如下
        - i.static 开头的类属性，如 defaultProps、propTypes。
        - ii.构造函数，constructor。
        - iii.getter/setter（还不了解的同学可以暂时忽略）。
        - iv.组件生命周期。
        - v._ 开头的私有方法。
        - vi.事件监听方法，handle*。
        - vii.render*开头的方法，有时候 render() 方法里面的内容会分开到不同函数里面进行，这些函数都以 render* 开头。
        - viii.render() 方法 
- 21.高阶组件：高阶组件就是一个函数（而不是一个组件），传给他一个组件，它返回一个新的组件;
- 22.context 某个组件只要往自己的 context 里面放了某些状态，这个组件之下的所有子组件都直接访问这个状态而不需要通过中间组件的传递。一个组件的 context 只有它的子组件能够访问，它的父组件是不能访问到的，你可以理解每个组件的 context 就是瀑布的源头，只能往下流不能往上飞。
- 23.纯函数：一个函数的返回结果只依赖于它的参数，并且在执行过程里面没有副作用，我们就把这个函数叫做纯函数;

```
