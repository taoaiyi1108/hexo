---
title: React原型解析（JS组件模块化-复用）
date: 2018-03-05 15:25:18
tags: React
categories: 编程
---
使用ES6中的class和字符串模板，使用继承方法，模块化页面

<!-- more -->

#### 介绍

- 组件化可以帮助我们解决前端结构的复用性问题，整个页面可以由这样的不同的组件组合、嵌套构成。
- 一个组件有自己的显示形态（下面的 HTML 结构和内容）行为，组件的显示形态和行为可以由数据状态（state）和配置参数（props）共同决定。数据状态和配置参数的改变都会影响到这个组件的显示形态。
- 当数据变化的时候，组件的显示需要更新。所以如果组件化的模式能提供一种高效的方式自动化地帮助我们更新页面，那也就可以大大地降低我们代码的复杂度，带来更好的可维护性。

- 学习更多，移步大牛 <b>胡子大哈</b> 的[技术文档](http://huziketang.com/books/react/)

#### 源码

``` html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>React-3-抽象出公共组件类</title>
</head>
<body>
    <div class='wrapper'></div>
</body>
<script type="text/javascript">

class Component {
	constructor(props = {}){
		this.props = props;
	}
	setState(state){
		const oldEl = this.el;
	    this.state = state;
	    this.el = this.renderDOM();
	    if(this.onStateChange) this.onStateChange(oldEl,this.el)
	}
	renderDOM(){
		this.el = creatDOMFromString(this.render());
		if(this.onClick){
			this.el.addEventListener('click',this.onClick.bind(this),false)
		}
		return this.el;
	}
}

const creatDOMFromString = (domString) =>{
	const div = document.createElement('div');
	div.innerHTML = domString;
	return div;
}

/*===================================================*/
class LikeButton extends Component{
	constructor(props){
		super(props)
		this.state = {isLiked:false}
	}

	onClick(){
		this.setState({
			isLiked: !this.state.isLiked
		})
	}

	render(){
		return `<button class='like-button' style='background-color:${this.props.bgColor}'>
				 <span class='like-text'>${this.state.isLiked?'取消':'点赞'}</span>
          		 <span>👍</span>
			</button>`
	}
}

class RedBlueButton extends Component{
	constructor(props){
		super(props)
		this.state = {
			color:'red'
		}
	}

	onClick(){
		this.setState({
			color:'blue'
		})
	}
	render(){
		return `<div style='color:${this.state.color};'>${this.state.color}</div>
		`
	}
}

/*============================================*/
const mount = (component,wrapper) =>{
	wrapper.appendChild(component.renderDOM());
	component.onStateChange = (oldEl,newEl)=>{
		wrapper.insertBefore(newEl,oldEl);
		wrapper.removeChild(oldEl);
	}
}
/*==============================================*/
const wrapper = document.querySelector('.wrapper');

mount(new LikeButton({bgColor:'red'}),wrapper)
mount(new RedBlueButton(),wrapper)
</script>
</html>

```