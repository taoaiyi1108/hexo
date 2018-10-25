---
title: vue中路由跳转及传参总结
date: 2018-02-26 13:46:56
tags: vue 
categories: 编程
---

路由跳转（router-link）

<!-- more -->

```html

<router-link to='/content'></router-link>	<!-- 此时的to不需要 v-bind：to -->

<router-link :to='{path:'/content'}'></router-link>	<!-- 此时的to需要 v-bind：to -->

```

#### 路由传值

 - query 传值（暴露参数 `http://localhost:8080/#/home?id=123&number=456`）

 ```javascript
	// router
    {
        path:'/content',
        component:content
    }
	//app.vue 
    <router-link :to='{path:'/content',query:{id:123,number:msg}}'></router-link>
    data(){
        return{
            msg:456
        }
    }
	//content 组件中接受参数
    this.$route.query  //对象{index:123,number:456}
    this.$route.query.id  // 123
 ```

 - params 传值（暴露参数 `http://localhost:8080/#/home?id=123&number=456`）

 ```javascript
	// router
    {
        path:'/content',
        component:content
    }
    //app.vue 
    <router-link :to='{path:'/content',params:{id:123,number:msg}}'></router-link>
    data(){
        return{
            msg:456
        }
    }
	//content 组件中接受参数
	this.$route.params  //对象{index:123,number:456}
	this.$route.params.id  // 123
 ```

  - params 传值（不暴露参数 `http://localhost:8080/#/home`）

 ```javascript
	// router
    {
        path:'/content',
        name:'content',  //name属性必须加
        component:content
    }
    //app.vue 
    <router-link :to='{name:'content',params:{id:123,number:msg}}'></router-link>
    data(){
        return{
            msg:456
        }
    }
	// content 组件中接受参数
	this.$route.params  //对象{index:123,number:456}
	this.$route.params.id  // 123
 ```
