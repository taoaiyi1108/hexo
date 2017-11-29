---
title: Vue-mint-ui-Picker的使用方法
date: 2017-11-29 10:38:06
tags:
	- vue
	- mint-ui
categories: 编程

---

之前自己做项目的时候用到了mint-ui的Picker模块，遇到了一些小的bug去网上查找。全都是些CV大法的段子手，把官网的文档copy过来就是自己的成果，完全没有一点的使用价值。

本人已经爬坑，那就来一个系统的教学，纯手工打造，希望对大家有所帮助！


首先mint-ui的官网地址：[mint-ui](http://mint-ui.github.io/#!/zh-cn) 提供了中文版的使用手册

话不多说直奔Picker模块主题

这是官方例子模块
<div align=center>
	![](http://p040q6o73.bkt.clouddn.com/image/boke/mint-ui/pickermint-ui-picker-1.gif)
</div>

我的项目的效果图
<div align=center>
	![](http://p040q6o73.bkt.clouddn.com/image/boke/mint-ui/pickermint-ui-picker-2.gif)
</div>
	
那么如何做到呢？
如果你按照手册一步一步的走下来，显示效果是这样的，完全和人家的效果图不一样啊，设置什么默认显示第几个什么的都没有效果
<div align=center>
	![](http://p040q6o73.bkt.clouddn.com/image/boke/mint-ui/pickermint-ui-picker-3.gif)
</div>
当你滑动滑块才会出现选中值高亮，css也会加上类名
<div align=center>
	![](http://p040q6o73.bkt.clouddn.com/image/boke/mint-ui/pickermint-ui-picker-4.gif)
</div>

其实细心的小伙伴会发现当你第一次进入picker页面会发现所有的滑块都会有一次上滑动，也就验证了为啥你的picker要滑动才可以出现选中高亮，这就是问题关键了，以下是本人的解决问题的想法：

参考官方例子最下面的自动上滑例子：[源码地址](https://github.com/ElemeFE/mint-ui/blob/master/example/pages/picker.vue)
```js
<script>
	mounted() {
      this.$nextTick(() => {
        let step = 0;
        setInterval(() => {
          this.numberSlot[0].defaultIndex = step++;
          if (step > this.numberSlot[0].values.length - 1) {
            step = 0;
          }
        }, 1000);
      });
    }	
</script>
	看到没，动态改变defaultIndex的值达到高亮选中，不卖关子了，解决方法就是，当你的页面打开之后defaultIndex要有一次动态改变

```

我的项目源码
核心点在：vue的created沟子在mounted之前执行，那么就会有一个时间差，在created中设定defaultIndex值，再到mounted改变defaultIndex的值（默认要显示的值的序号由1开始）
```html
 <div class="riliSilder">
	  <span class="sliderTit">In</span>
	  <mt-picker :slots="yearSlot" @change="onYearChange" :visibleItemCount ='visibleItemCount' class="slider" :itemHeight = '38' ></mt-picker>
</div>
```
```js
<script>
  import Vue from 'vue'
  import { Picker } from 'mint-ui';
  Vue.component(Picker.name, Picker);
  export default {
      data(){
        return{
          year: 2017,
          yearSlot: [{
            flex: 1,
            values: [],
            className: 'slot1',
            defaultIndex:0
          }],
          visibleItemCount:3,
          genderInfo:{}
        }
      },
      created(){
        this.yearSlot[0].defaultIndex = 1;
        this.getYear()
        this.year = this.getYear()
      },
      methods:{
        back(){
          history.go(-1)
        },
        onYearChange(picker, values) {
          this.year = values[0];
          this.genderInfo.BORNYEAR = this.year;
          localStorage.setItem('genderInfo',JSON.stringify(this.genderInfo))
        }
        },
        /*创建年份数组*/
        getYear(){
          var year = [];
          var data = new Date();
          var nowYear = data.getFullYear();
          for(let i =1940;i<=nowYear;i++){
            year.push(i)
          }
          this.yearSlot[0].values = year

          if(this.genderInfo.BORNYEAR){
            this.step = year.indexOf(this.genderInfo.BORNYEAR)
          }else {
             this.step = year.indexOf(nowYear)
          }
        }
      },
      mounted(){
         this.$nextTick(() => {
             this.yearSlot[0].defaultIndex = this.step
          });
          this.genderSelect(this.genderInfo.gender)
        }
  }
</script>
	
```





	