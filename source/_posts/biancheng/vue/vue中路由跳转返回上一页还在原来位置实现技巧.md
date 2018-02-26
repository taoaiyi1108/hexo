---
title: vue中路由跳转返回上一页还在原来位置实现技巧
date: 2017-12-21 15:57:47
tags: vue
categories: 编程

---

```html
<div class="box">
	<ul>
		<li v-for="{item,index} in list" @click-native="sessionIndex(index)" ref="list">
			<router-link to="/content">路由</router-link>
		</li>
	</ul>
</div>
```
```javascript
export defalut{
	data(){
		return{
			routerIndex:''
		}
	},
	created(){
		this.routerIndex = sessionStorage.getItem('routerIndex');
		if(this.routerIndex){
          this.routerIndex = parseInt(this.routerIndex)
        }else {
          this.routerIndex = 0;
        }
	},
	methods(){
		sessionIndex(index){
			sessionStorage.routerIndex = index
		}			
	},
	beforeUpdate(){
        let _this = this;
        this.$nextTick( () => {
          if(_this.videoindex != 0){
            _this.$refs.list[_this.routerIndex].scrollIntoView(false);
            _this.routerIndex = 0;
          }
        })
      }
}
```

逻辑说明：
- v-for遍历 点击对应路由跳转时session存储对应了index
- 返回路由列表页面时，沟子created中session获取routerindex，存放到data数据中
- 沟子beforeUpdate时利用scrollIntoView方法完成滚动