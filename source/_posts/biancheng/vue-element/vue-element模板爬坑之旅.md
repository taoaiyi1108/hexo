---
title: vue-element模板爬坑之旅（持续更新）
date: 2018-01-05 15:07:13
tags: vue-element
categories: 编程

---
<div style="color: #5278C8;font-family:'楷体'">
	在此感谢PanJiaChen大神的帮助
</div>
####  目录结构
    ├── build                      // 构建相关  
	├── config                     // 配置相关
	├── src                        // 源代码
	│   ├── api                    // 所有请求
	│   ├── assets                 // 主题 字体等静态资源
	│   ├── components             // 全局公用组件
	│   ├── directive              // 全局指令
	│   ├── filtres                // 全局 filter
	│   ├── icons                  // 项目所有 svg icons
	│   ├── lang                   // 国际化 language
	│   ├── mock                   // 项目mock 模拟数据
	│   ├── router                 // 路由
	│   ├── store                  // 全局 store管理
	│   ├── styles                 // 全局样式
	│   ├── utils                  // 全局公用方法
	│   ├── vendor                 // 公用vendor
	│   ├── views                  // view
	│   ├── App.vue                // 入口页面
	│   ├── main.js                // 入口 加载组件 初始化等
	│   └── permission.js          // 权限管理
	├── static                     // 第三方不打包资源
	│   └── Tinymce                // 富文本
	├── .babelrc                   // babel-loader 配置
	├── eslintrc.js                // eslint 配置项
	├── .gitignore                 // git 忽略项
	├── favicon.ico                // favicon图标
	├── index.html                 // html模板
	└── package.json               // package.json
#### 逐步解读
```
- 登录
	- vue位置 src/views/login/index.vue
	- 逻辑
		- 密码input的键盘enter事件和点击submit同为一样的事件函数（handleLogin），
		- 在函数（handleLogin）中使用this.$store.dispatch('Login')，调用vuex仓库中存放的方法Login（store/modules/user.js）实现登录
		- 而Login中实际上使用了login方法（api/login.js），
		- login方法中的数据请求使用的是utils中封装的request（utils/request.js）方法，
			- request方法中需要注意的是：
				- 登录状态码需要和后台就行约定
					- 登录成功时的code需要和后台约定（vue-element中使用20000）
					- 50008:非法的token; 
					- 50012:其他客户端登录了;  
					- 50014:Token 过期了;
				- 判断登录过期的token 也在request中修改
		- 在login方法请求成功后使用setToken（utils/auth.js）将token存放到cookie中，同时使用vuex的SET_TOKEN方法将token存入state中
		- 登陆成功后在函数（handleLogin）的回调中使用vue的$router.push跳转路由到主页面
```