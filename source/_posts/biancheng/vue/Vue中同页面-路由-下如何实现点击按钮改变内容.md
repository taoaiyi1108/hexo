---
title: Vue中同页面(路由)下如何实现点击按钮改变内容
date: 2018-11-13 18:06:38
tags: 
    - vue
categories: 编程
---


<!-- more -->

##### 很多同学应该有过这样的经历
- 通过点击电影list页面的一项，通过URL传值把电影的ID传到电影的play页面，通过ID获取电影详情，播放电影；
- 在电影播放页面下面又会有推荐电影的list，用户点击list中的item电影，页面播放当前点击的电影；
- 然而很多同学很容易完成上面的需求，但是，当用户刷新页面后，播放的电影又成了第一次进入时的电影，原因是，在页面刷新的时候Vue组件的生命周期重新加载，此时获取电影详情的方法传入的ID是你在URL中带过了的电影ID，你点击按钮播放的推荐list中的点ID根本就没有记录；
- 此时为了来解决这个问题，有人会想到本地缓存，当然，用本地缓存是可以解决用户刷新播放电影回跳的BUG，但依旧会有一个问题就是，用户看到一个电影看到一半有事想退出，在没有用户登录的游客身份下，他选择了收藏该页面；
- 那么问题来了，他下次进入这个页面是不是还是和之前页面刷新一样的问题，页面还是用url上的ID获取电影的详情 ，不是用户收藏时的电影
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/vue/4.png)

##### 说到这里很多同学应该已经明白了，问题的关键就在url上，只要用户点击推荐list电影，URL就要改变，DI变成当前播放的电影的ID；


##### 下面我们就谈谈使用Vue如何实现这样的逻辑
- 先说有问题的逻辑（啰嗦一下免的有些同学不太懂）
```html
<template>
    <div class="content">
        <video src="url" class="play"></video>
        <ul>
            <li class="list-item" v-for="(v,i) in videolist" :key="i" @click="clickItem(v.id)"></li>
        </ul>
    </div>
</template>
<script >
export default {
    name:"play",
    data(){
        videodata:{},
        videolist:[]
    },
    methods:{
        /*获取当前要播放的电影详情*/
        getvideo(id){
            ajax.get(url).then(resolve => {this.videodata = resolve.data} );
        },
        /*推荐电影list数据*/
        getVideolist(){
            const category = this.$route.query.category;
            ajax.get(category).then(resolve => {this.videolist = resolve.data} );
        },
        /*点击推荐电影list，播放点击的电影*/
        clickItem(id){
            this.getvideo(id);
        },
        /*获取URL上的video ID*/
        getVideoId(){
            const videoId = this.$toure.query.id;
            /*此处判断缓存中是否有videoId，解决刷新后当前播放的video回调*/
            this.getvideo(videoId);  
        }
    },
    created(){
        this.getVideoId();
        this.getVideolist();
    }
}
</script>
```
- 正确的逻辑是怎样的呢？
```javascript
/*我们只需点击推荐list中电影时，重置路由，然后在watch中监控路由是否变化就可以了*/
methods:{
    /*点击推荐电影list，播放点击的电影*/
    clickItem(id){
        this.$router.push({path:'/videoplay',query:{id}});
    },
},
watch:{
    "$route"(to,from){
        if(to.query.id != from.query.id){
            /*获取URL上的video ID*/
            this.getVideoId(){
                const videoId = this.$toure.query.id;
                /*此处判断缓存中是否有videoId，解决刷新后当前播放的video回调*/
                this.getvideo(videoId);  
            }
        }
    }
}
/*
这样路由的的参数id也改变了,当前页面数据也刷新了*/

```

###### 以上是个人思路，如有不对请指出，<a href="/about">@我</a>
