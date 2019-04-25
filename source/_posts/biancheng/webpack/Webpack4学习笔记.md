---
title: Webpack4学习笔记
date: 2019-04-09 22:34:19
tags: webpack
categories: 编程
---

<!-- more -->
##### Webpack核心定义
- webpack是一个模块打包工具
- webpack支持 `ES moudles 模块引入方式 Common JS 模块引入规范 CMD ADM`

##### Webpack环境搭建
- `npm init` 初始化项目(询问需要手动配置) 符合node规范 `npm init -y` 默认配置
- 安装webpack
    - 全局安装 `npm install webpack webpack-cli -g` 不推荐，所以移除 `npm uninstall webpack webapck-cli -g`
    - 局部安装(项目内安装) `npm install webpack webpack-cli --save-dev || -D`
        - 查看webpack版本 `npm info webpack`
        - 安装指定版本 `npm install webpack@4.1.2 webpack-cli -D`
    - 安装webpack-cli的作用能够在终端使用完webpack的命令

##### 导入导出
```javascript
    /*ES module 语法 */
    import --> export default   // 底层是一个静态引入方式
    /* CommonJS 语法 */
    require() --> module.exports // 底层是一个动态的引入方式
```

##### Webpack 目录结构


##### Webpack配置文件 `webpack.config.js`
- `npx webpack --config webpack.config.js` 指定webpack以webpack.config.js文件为配置文件进行项目打包
```javascript
const path = require('path'); // node 的绝对路径

module.exports = {
    mode: 'production', // 打包的模式 默认是production 打包后 js是压缩的  还有 development
    // entry: './src/index.js', // 打包入口文件
    entry: {
        main: './src/index.js', // 另一种写法
    },
    output: {
        filename: 'bundle.js', // 打包后输出的文件名称
        path: path.resolve(__dirname, 'dist'), // node中的变量__dirname 指当前webpack.config.js所在的目录的路径 打包后生成dist文件下有个bundle.js 
    }
}
```

##### npm scripts
- 没有在 package.json 中的scripts中配置时我们使用 `npx webpack ` 进行打包操作
```json
{
    ...
    "scripts": {
        "bundle":"webpack", // 此时就可以执行命令 npm run bundel 来打包项目了
    },
    ...
}
```

##### Webpack 打包输出 log
```
Hash: ecceaa813d657c671fc8  /* 本次打包唯一的hash值 */
Version: webpack 4.29.6     /* 本次打包使用的webpack的版本 */ 
Time: 144ms                 /* 本次打包耗时 */  
Built at: 2019-04-10 22:41:29
    Asset       Size  Chunks             Chunk Names  /* Asset打包后文件名称 Size 文件大小  Chunks打包后js文件的ID  ChunkNames 打包后js文件的名字 */ 
bundle.js  930 bytes       0  [emitted]  main
Entrypoint main = bundle.js /* 入口文件 */
[0] ./src/index.js 0 bytes {0} [built]

```

##### loader
-  配置 `webpack.config.js`
- loader就是一个打包方案
```javascript
    /* ..... */
    module: {
        rules: [
            {
                test: /\.jpg$/, /* 图片打包 */
                use: {
                    loader: 'file-loader', /* npm install file-loader -D 安装file-loader */
                }
            }
        ]
    }
    /* ..... */
```
- loader 打包静态资源
```javascript
    rules: [
        {
            test: /\.(jpg|jpeg|jpg|gif)$/, // 文件格式
            use: {
                loader: 'url-loader', // 文件  url-loader 替代 file-loader   url-loader会把图片打包出base64
                options: { // 为文件打包过程中配置
                    // 配置语法 placeholder 占位符
                    name: '[name].[ext]', // 打包后文件的名称（此处不修改文件名称） name 文件名称 ext 文件后缀
                    outputPath: 'images', // 文件打包后存放的路径
                    limit: 2048, //将小约2048字节的图片打包出base64
                }
            }
        },
        {
            test:/\.(css|scss)/,
            use:[ // 多个loader use是一个数组  loader执行的顺序是由下至上，由左到右
                'style-loader',
                // 'css-loader',
                {
                    loader:"css-loader",
                    options: {
                        importLoaders: 2, // index.sass @import 其他sass时使用，否则引入的sass不会执行 sass-loader和postss-loader
                        modules: true, // 开启css模块化打包
                    }
                },
                'sass-loader', //sass-loader 需要安装 npm install sass-loader node-sass
                'postcss-loader', // css3兼容前缀添加的loader 需要创建postcss.config.js 配置文件
            ]
        },
        /* 字体图标打包 */
        {
            test: /\.(eto|ttf|svg)$/,
            use: ['file-loader']
        }
    ]
```

##### plugins(插件)
- `html-webpack-plugin` 
```javascript
    const HtmlWebpackPlugin = require('html-webpack-plugin');
    const CleanWebpackPlugin = require('clean-webpack-plugin');
     plugins: [
        new HtmlWebpackPlugin({
            template: 'src/index.html', // 以index.html为模板生成html
        }), // 会在webpack打包后自动生成index.html 文件，并且将打包的js自动引入html中
        new CleanWebpackPlugin({
            // cleanOnceBeforeBuildPatterns:[path.resolve(__dirname,'dist')],
            cleanStaleWebpackAssets: true
        }), // 打包前 删除 dist目录中的文件 接收的是一个对象
    ],
```
##### Entry(入口) Output(出口)
```javascript
    // entry: './src/index.js', // 打包入口文件
    entry: {
        main: './src/index.js', // 另一种写法
        sub: './src/index.js'
    },
    output: {
        // publicPath: './', // 打包后index.html中引入的js的src会自动添加publicPath地址
        // filename: 'bundle.js', // 打包后输出的文件名称  没有指定自动根据entry的key值删除打包后的名称，或者使用占位符
        filename: '[name].js', // name 指的就是 entry的key值
        path: path.resolve(__dirname, 'dist'), // node中的变量__dirname 指当前webpack.config.js所在的目录的路径 打包后生成dist文件下有个bundle.js 
    },
```

##### SourceMap
- 映射，支出代码异常的位置
```javascript
    /* 开发环境推荐 */
    mode:'development',
    devtool: 'cheap-module-eval-source-map',
    /* 生产 可以不用 也可以配置 */
    mode: 'production',
    devtool: 'cheap-module-source-map'
```

##### WebpackDevServer
```javascript
    /* webpack.config.js */
    devServer: {
        contentBase: './dist',
        open: true, // 自动打开浏览器 自动访问地址
        port: 8080, // 端口号
    },
    /* package.json */
    scripts: {
        "start":"webpack-dev-server"
    }
```

##### Hot Module Replacement 热模块更新
```javascript
    /* webpack.config.js */
    const webpack = require('webpack');
    devServer: {
        contentBase: './dist',
        open: true, // 自动打开浏览器 自动访问地址
        port: 8080, // 端口号
        hot: true, // HMR 热加载
        hotOnly: true // html内容不生效浏览器不刷新  可选项
    },
    plugin: [
        new webpack.HashedModuleIdsPlugin(), // 热模块更新
    ],
    /* index.js */
    // 模块热替换  如果 print.js 这个文件发生改变就执行后面的代码
    if (module.hot) {
        module.hot.accept('./print.js', function () {
            document.body.removeChild(element);
            element = component(); // 重新渲染 "component"，以便更新 click 事件处理函数
            document.body.appendChild(element);
        })
    }
```

##### 使用Bable处理ES6
- `npm install --save-dev babel-loader @babel/core` 连接结babel和es6
- `npm install @babel/preset-env --save-dev` 转正实现 es6 转 es5  包含左右es6转es5的规则
- `npm install --save @babel/polyfill` 给低版本浏览器 补充ES6的函数，方法
- 业务代码配置
    ```javascript
        /* webpack.config.js */
        module: {
            rules: [
            {
                    test: /\.js$/,
                    exclude: /node_modules/, //exclude 排除
                    loader: "babel-loader",
                    options: {
                        presets: [
                            [
                                "@babel/preset-env",
                                {
                                    useBuiltIns: 'usage', // 添加es6语法 按需引入  import "@babel/polyfill" 自动引入
                                    corejs: 3,
                                    targets: {  // 浏览器版本
                                        edge: "17",
                                        firefox: "60",
                                        chrome: "67",
                                        safari: "11.1",
                                    },
                                }
                            ]
                        ]
                    }
                }, 
            ]
        }
        /* index.js */
        import "@babel/polyfill"; // 所有es6的语法都会打包 打包后js文件会很大  开发组件库不建议按此方法使用 polyfill会污染全局环境
    ```
- 开发组件库时
    - `npm install --save-dev @babel/plugin-transform-runtime`
    - `npm install --save @babel/runtime`
    ```javascript
        module: {
            rules: [
            {
                    test: /\.js$/,
                    exclude: /node_modules/, //exclude 排除
                    loader: "babel-loader",
                    options: {
                        plugins:[
                            "@babel/plugin-transform-runtime",
                            {
                                "absoluteRuntime": false,
                                "corejs": 2, // corejs 值为2的时候 需要安装 npm install --save @babel/runtime-corejs2
                                "helpers": true,
                                "regenerator": true,
                                "useESModules": false
                            }
                        ]
                    }
                }, 
            ]
        }
    ```
- 提取babel options 配置 创建`.babelrc`文件 把options中的代码剪切过去即可
```javascript
    {
        "presets": [
            [
                "@babel/preset-env",
                {
                    "useBuiltIns": "usage", // 添加es6语法 按需引入
                    "corejs": 3,
                    "targets": { // 浏览器版本
                        "edge": "17",
                        "firefox": "60",
                        "chrome": "67",
                        "safari": "11.1",
                    },
                }
            ]
        ]
    }
```

##### Webpack 实现React框架代码的打包
- `npm install react react-dom --save`
- `npm install --save-dev @babel/preset-react`
```javascript
    /* index.js */
    import "@babel/polyfill";
    import React, { Component } from 'react';
    import ReactDom from 'react-dom';

    class App extends Component {
        render() {
            return (
                <div>Hello World</div>
            )
        }
    }

    ReactDOM.render(<App />, document.getElementById('root'));

    /* .babel */
    {
        "presets": [
            [
                "@babel/preset-env",
                {
                    "useBuiltIns": "usage", // 添加es6语法 按需引入
                    "targets": { // 浏览器版本
                        "edge": "17",
                        "firefox": "60",
                        "chrome": "67",
                        "safari": "11.1",
                    },
                }
            ],
            "@babel/preset-react"
        ]
    }
```

##### Tree Shaking 
- 只打包使用到的依赖模块  生成环境生效
- Tree Shaking 只支持 ES Moudle 的 引入方法 也就是 import 的方式 
```javascript
    /* webpack.config.js */
    {
        mode: "development",  // 开发环境需要配置
        optimization: {
            usedExports: true
        }
    }
    /* package.json */ 
    "sideEffects": false, // tree shaking 对所有的模块进行处理
    /* 或者 */
    "sideEffects": ["@babel/polyfill", "*.css"] // 忽略掉不处理
```

##### Development 和 Production 模式区分打包
- 新建 `webpack.dev.js` `webpack.prod.js` `webpack.common.js`
```javascript
    /* package.json */
    "scripts": {
        "dev": "webpack-dev-server --config webpack.dev.js",
        "build": "webpack --config webpack.prod.js",
    },
    /* 打包配置统一放置在build文件中 */
    "scripts": {
        "dev": "webpack-dev-server --config ./build/webpack.dev.js",
        "build": "webpack --config ./build/webpack.prod.js",
    },
```
- `npm install webpack-merge -D` 合并 webpack.common.js 
```javascript
    /* webpack.dev.js */
    const webpack = require('webpack');
    const merge = require('webpack-merge');
    const commonConfig = require('./webpack.common');
    const devConfig = {}
    module.exports = merge(commonConfig, devConfig);
    /* webpack.prod.js */
    const merge = require('webpack-merge');
    const commonConfig = require('./webpack.common');
    const prodConfig = {}
    module.exports = merge(commonConfig, prodConfig);
```