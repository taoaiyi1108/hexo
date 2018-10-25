---
title: vue 打包后背景图片路径错误
date: 2017-12-11 14:35:42
tags: vue
categories: 编程

---
说到这就得提及vue中图片的存放位置
- src/assets 
	- 图片放在此文件夹下面，.vue 中使用时 `<img src='./assets/paly.png'/>`
- static/img
	- 图片放在此文件夹下面， .vue 中使用时 `<img v-bing:src='imgPath'/>`
	- 图片的路径 可以用 `import imgPath from '../static/img/play.png'`适合于单个图片
	- 图片过多时建议在data中
```javascript
    data(){
         return{
             imgs:['static/paly.png','static/go.png'] // 不用../../ 直接 static 就可以了
         }
     }
     
    <img :src="imgs[0]"/>
```

配置文件中更改打包路径

- 更改 build/utils.js 文件中 ExtractTextPlugin 插件的options 配置：
```javascript
	if (options.extract) {
	  return ExtractTextPlugin.extract({
	    use: loaders,
	    publicPath: '../../',         // 注意配置这一部分，根据目录结构自由调整
	    fallback: 'vue-style-loader'
	  })
	} else {
	  return ['vue-style-loader'].concat(loaders)
	}
```
- config/index.js 文件中的build下的assetsPublicPath（如果上面的更改不起作用在修改此处）
```javascript
	assetsSubDirectory: 'static',
    assetsPublicPath: './', //此处更改
    proxyTable: {},
```

- 一切准备就绪就可以`npm run build`就行打包了 默认打包出来的在dist文件下面，如需要更改
-  config/index.js 文件中的build下 
```javascript
	build: {
	    // Template for index.html
	    index: path.resolve(__dirname, '../dist/index.html'), // dist 要和下面的文件名称保持一致
	
	    // Paths
	    assetsRoot: path.resolve(__dirname, '../dist'), // dist 打包出来的文件名称
	    assetsSubDirectory: 'static',
	    assetsPublicPath: '/',
	}
```