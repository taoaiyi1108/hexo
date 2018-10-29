---
title: Vue中实现上拉加载
date: 2018-10-26 11:14:16
tags: vue
categories: 编程
---
虽说Vue相关的UI框架都提供了上拉加载的功能，但在实际开发过程中我们的需求是不断变化的，需要自定义，那么现在就来讲解一下Vue中上拉加载现的思路

<!-- more -->
##### 页面布局
```html
<template>
    <div class="container" ref="list">
        <ul class="wrap">
            <list class="item" v-for="(v,i) in data" :key="i">
                <p>text</p>
            </list>
        </ul>
    </div>
</template>
```
##### 逻辑处理
- 1、全局页面
    ```javascript
    export default {
        name:'List',
        data(){
            return {
                data:[],
                pageSize:5,
                getMoreFlag:true,/*节流阀*/
            }
        },
        /*当前页面离开时移除window身上的scroll事件*/
        beforeRouteLeave(to,from,next){
             window.removeEventListener('scroll', this.scrollMore);
             next()
        },
        methods:{
            /*获取数据*/
            getdata(pageSize){
                var url = 'api接口'+pageSize;
                Ajax.jsonp(url).then( response =>{
                    this.getMoreFlag = true;
                    this.data = response.data;
                })
            },
            /*下拉加载*/
            getmore(){
                if(this.getMoreFlag){
                    this.getMoreFlag = false;
                    this.pageSize += 5;
                    this.getdata(this.pageSize);
                }else {
                    return;
                }
            },
            /*监控windowscroll*/
            scrollMore(e){
                /*抓取到 list dom元素 才开始监控 scroll*/
                if(this.$refs.list){
                    const windowHeight = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight || 0;
                    var windowScrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
                    var contenH = Math.floor(parseFloat(window.getComputedStyle(this.$refs.list).height));
                    /*内容页大于视口高度*/
                    if(contenH > windowHeight){
                        if(contenH - windowScrollTop <= windowHeight){
                            /*加载更多*/
                            this.getmore();
                        }
                    }else {
                        console.log('loading');
                    }
    
                }else {
                    console.log('no page');
                }
            },
        },
        mounted(){
            /*确保DOM加载完成才给window挂在scroll事件*/
            this.$nextTick( () =>{
                window.addEventListener('scroll', this.scrollMore);
            })
        },
        created(){
            this.getdata(this.pageSize);
        }
    }
    /*注意点说明
    * 1.因为window是一个全局的变量，在A页面给window添加的监听事件在B页面也是可以被调用的，这就会导致程序在B页面的时候报错；
    *   为了把window上的监听事件在当前页面私有化，避免上述的问题，我们可以采用VueRouter的组件内钩子beforeRouterLeave，在离开当前组件
    *   页面时移除window添加的监听事件
    * 2.同时也为了避免本页面监听事件出现异常，我们要确保当前页面的DOM元素都加在完成，在Vue的mounted钩子中给window挂架监听事件，
    *   同时再加一层判断当前listDOM有的时候才会触发scroll中的逻辑，否则就让他跑空，避免其他的错误发生
    * 3.记得添加节流阀，只有当上一次请求完成数据完成后才可以进行下一次的下拉请求
    * */
    ```
- 2、局部加载
    ```html
    <div class="wrap" ref="wrap">
        <ul class="list" ref="list" >
            <li class="item"></li>
        </ul>
    </div>
    ```
    ```css
    .wrap {
      height: 500px;
      overflow: scroll; /*这里注意必须要有，不然子元素会把父级元素撑开*/
    }
    .scrool-box {
      height: 2000px;/*这里只是举个例子，子元素比父元素高，父元素就会有滚动条*/
    }
    ```
    ```javascript
    /*基本的逻辑和全局页面加载是一样的，只是在scrollMore里面对于DOM样式的计算上不太一样，下面我们就单讲这里*/
    export default {
      /*********/
        scrollMore(e){
          if(this.$refs.wrap){
             var wrap = this.refs.wrap;
             var list = this.refs.list;  
              /*scrillHeight 整个元素的高度（包括带滚动条的隐蔽的地方） 换句话说就是元素包括内容的高度
              offsetHeight 任何一个元素的高度包括边框和填充，但不是边距  换句话说就是元素本身的高度*/
              if(wrap.scrollHeight - wrap.scrollTop == wrap.offsetHeight){
                  /*加载更多*/
                  this.getmore();
              }else {
                  console.log('loading');
              }
          }
        }
      /**********/
    }
    ```
    
- 以上是本人自己总结如果有错误或者表述不当还请移步 <a href="/about">About</a> 页面邮件指出，谢谢!


