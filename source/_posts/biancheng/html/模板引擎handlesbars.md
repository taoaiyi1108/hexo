---
title: 模板引擎handlesbars
date: 2017-12-11 15:09:57
tags: html
categories: 编程

---

虽说现在前端开发都是用各大前端技术框架，对于domo渲染都有着自己的一套模板，但如果你维护老的产品时依然会发现模板引擎，今天就来说一说模板引擎handlesbars最基本是使用方法

- 模板引擎 说白了就是遍历数组对象的一种方法，提高网页加载速度 类似于vue中的v-for  angular中的ng-for
- 引入js文件`<script type="text/javascript" src="./js/handlebars.min.js"></script>`
- 开始使用 （基于JQ）
```html
	<div class="videoList">
		<ul class="videoListWrap">
			<script id="video-template" type="text/x-handlebars-template">
			{{#each modelList}}
				<li class="videoItem">
					<input type="hidden" value="{{identification}}" class="videoId">
					<div class="wrap">
						<!-- 不需要data.thumbnail,直接在{{}}中添加需要遍历的属性名称-->
						<img src="{{thumbnail}}" alt="" class="videoThum">
						<img src="img/play.png" alt="" class="play"> 
					</div>
				</li>
			{{/each}}
			</script>
		</ul>
	</div>
```
```javascript
	/*获取视频列表*/
	var videoTemplate = Handlebars.compile($("#video-template").html()); //定义获取对应的模板
	function getVideolist(videoListId){
		var id;
		$.ajax({
			type : "get",
			url : url,
			dataType : "jsonp",
			jsonp : "callback",
			success : function(data) {
				$('.videoListWrap').html(videoTemplate(data)); // 获取数据后 赋值给模板
			}
		});

```
