---
title: vue获取dome元素爬坑之旅
date: 2018-01-27 15:58:22
tags: vue
categories: 编程

---
### vue中获取dome元素的方法(需要注意vue的钩子函数触发时间，mounted之后dome才被实例化)

- ref方法

```html

	<div ref='dome'></div>

```

```javascript

	mounted(){
		this.$refs.dome //就可以获取到
	}
	methods：{
		go(){
			this.$refs.dome //就可以获取到
		}
	}

```

- 点击时（click）获取当前点击元素和当前点击元素的子元素

```html
	
	<div @click='go($event)'></div>

```

```javascript

	methods:{
		go(e){
			el.currentTarget //获取当前父元素
 	 		el.target //获取当前点击的目标元素
		}
	}

```

- 获取v-for动态加载的元素（ajax获取数据宣言ul）

```html

	<div class="wrap">
		<ul class="box">
			<li class="lis" v-for='item in datalist'></li>
		</ul>
	</div>

```

```javascript

	<script>
		data(){
			return {
				datalist:[]
			}
		},
		created(){
			this.getlist();
		},
		methods:{
			getlist(){
				var _this = this;
				this.#http.get(url).then( (res)  =>{
						_this.datalist = res.body;
						_this.$nextTick( () =>{
								var lis = document.querySelectorAll('.lis');
								// lis 就是ul下的li
							})
					})
			}
		}
	</script>

```

- 遇到用v-if或v-show控制dome显示隐藏时要注意；如果你要操作样式，最好选用v-show来控制，v-if=false时 根本就没有dome 所以你永远获取不到dome