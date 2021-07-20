---
title: 浅谈vue-cli3.x的multi-page模式
date: 2021-04-01 09:36:52
tags: element
categories: 编程
---
学以致用吧，总结归纳。

<!-- more -->

### 写在开头

开始之前，说点题外话，见谅。

18年吧，当时入职的时候，公司正在开发一个建筑方面的CMS项目，进去的时候，用的是vue-cli2.x启的项目。开发完成度已经接近百分之七十了，但是也遇到了很多问题，其中最头疼的问题就是:

- 项目启动慢
- 生产环境首屏加载慢，当然了这也是SPA（单页面）项目的通病，白屏现象严重影响产品体验感

在磨合一周后主管让我去解决这个问题，（谁让咱面试的时候吹的水太多了呢，🐕），不过也不用担心，面向百度乃开发神技，哈哈，开个玩笑。

总结分析了下，上述项目问题的原因我也略知七八：

主要是项目庞大，好家伙，`.vue`文件都快`200+`，一个`npm run dev`（vue-cli2.x的启动命令）下去，`node`开动了，结合`webpack`的配置，哗啦哗啦，一股脑把加载的文件都要过一遍，加上七七八八的附件，“这头牛”有跑的快的理由吗，项目启动自然就慢。

还有就是前端展示上是划分子系统，不同的子系统不同的菜单，但机制上还是揉在一起，只是通过逻辑去做了hidden。

然后就是`build`之后，`chunk-vendors.js` 超2MB, 熟悉单页面加载机制的同学应该都明白了，首屏加载慢的原因了吧。

```code
我理解的SPA渲染机制：

    第一步： http请求，server返回index.html，这个index.html很简单，就是一个 <div id="app"></div>，这也是SPA不利于SEO的原因

    第二步：浏览器（broswer）加载渲染index.html，在渲染过程中，就要去加载执行chunk-vendors.js(Vue的逻辑)

    第三步：执行chunk-vendors.js等一系列Vue逻辑的js文件，创建VDOM啊什么的

    第四步：找到挂载点 #app，将VDOM装换成真实DOM插入DOM树，页面展示出来

    首屏加载慢就出在了二、三步，加载vue逻辑文件要时间吧，vue逻辑文件执行要时间吧，在此时间过程中，如果index.html不做loading处理，其实就是一个空的div页面，白屏了。

    当然了，也跟Broswer设备性能有一定的关系，不过现在不需要担心了，古董机很少了。想想向前辈，心痛啊。
```

怎么感觉和标题不符啊，瞎扯的远了。U•ェ•*U

好，既然问题明了，那就着手解决。

我给出的方案：

- 升级 `vue-cli2.x` 至 `vue-cli3.x`， 新的东西自然有牛逼之处
- 使用 `vue-cli3.x`中的 `multi-page`模式，也就是多页面，拆分项目。

项目升级迁移在此就不过多赘述，因为每个人的手法不一，咱自己也是依葫芦画瓢，就不要误人子弟了。

直接上前后效果对比

![普通模式打包后](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/vue/multi-page-1.png)
![multi-page模式打包后](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/vue/multi-page-2.png)

可以明显的看到`multi-page`模式打包后, 子系统都有着自己独立的文件目录，独立的`index.html`

看看线上：
![multi-page模式打包后](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/vue/multi-page-3.png)
![multi-page模式打包后](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/vue/multi-page-4.png)

明白了吧，除了公用的js之外，不同子系统加载对应自己的js逻辑文件， 这样就可以缩减先前说到的二、三步骤的时间，如果拆分做到极致，是不是可以就秒加载了，问题不就解决了吗？ 🤭


### multi-page实现过程

如果不熟的`vue-cli3.x`的移步这里，[传送门](https://cli.vuejs.org/zh/config/#pages)，核心就是`vue-config.js`中的`pages`，最终要是实现这样的

```js
modelu.exports = { 
     pages: {
        index: {
            // page 的入口
            entry: 'src/pages/index/main.js',
            // 模板来源
            template: 'src/pages/index/index.html',
            // 在 dist/index.html 的输出
            filename: 'index.html',
            chunks: ['chunk-vendors', 'index']
        },
        task: {
            // page 的入口
            entry: 'src/pages/task/main.js',
            // 模板来源
            template: 'src/pages/task/index.html',
            // 在 dist/index.html 的输出
            filename: 'task.html',
            chunks: ['chunk-vendors', 'task']
        }
    }
}
```

也就是`build`后就有了`index、task`两个文件夹， 但是问题又来了，我多一个子系统，我就要去这里维护一下，是不是很麻烦，有没有自动识别`pages`文件夹下子文件夹个数来自动组装`pages`对象数据。

作为一个老油条，这个问题肯定要解决的啦。

老样子，直接上成品：

![项目目录结构](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/vue/multi-page-5.png)

```js
    // vue-config.js
    const path = require('path')
    const fs = require('fs')
    const resolve = dir => path.resolve(__dirname, dir)

    const config = {
        entry: 'main.js',
        html: 'index.html',
        pagesRoot: resolve('src/pages')
    }

    const getSubSystem = () => {
        const allSubSystem = []
        const findAllSubSystem = (source, allSubSystem) => {
            // 同步读取pages下的文件
            const files = fs.readdirSync(source) 
            for (let i = 0; i < files.length; i++) {
                const filename = files[i]
                // 组装文件路径
                const fullname = path.join(source, filename) 
                // 路径转化成 'bigint'型，便于下一步判断
                const stats = fs.statSync(source, filename) 
                // 判断上面读取的到文件是否为文件夹， 不是文件夹直接跳过循环，算是个性能优化吧
                if (!stats.isDirectory()) continue;
                // 判断目录是否真实有效， 也就是判断pages下的文件中有没有main.js
                if (fs.existsSync(`${fullname}/${config.entry}`)) { 
                    allSubSystem.push(fullname)
                }
            }
        }
        findAllSubSystem(config.pagesRoot, allSubSystem)
        return allSubSystem
    }

    const getPages = () => {
        const pages = {}
        getSubSystem().forEach(page => {
            // 通过文件夹路径来截取文件夹名称，作为pages的属性
            const filename = page.slice(config.pagesRoot.length + 1)
            pages[filename] = {
                entry: `${page}/${config.entry}`,
                template: `${page}/${config.html}`,
                filename: filename === 'index' ? config.html : `${filename}/${config.html}`
            }
        })
        return pages
    }

    const pages = getPages()

    module.exports = {
        // ...省略其他代码
        pages,
        // ...省略其他代码
    }

    /**
     * 整体思路就是： 
     * 1. 利用node的fs读取pages下的文件，筛选出符合条件的文件夹
     * 2. 便利筛选出来的文件夹list，转换成js的对象
     * 
     * 总结： 就是利用node完成了你手动完成的事情
    */
```


什么？写的太水了，没看懂，没事，在看一个高手的，[传送门](https://ddxg638.github.io/2019/08/11/vue-cli3-pages/)

### 继续搞

这样就完了吗，还没有，项目既然划分了不同的子系统，那么就意味着不是所有的子系统都会用到，就好比微服务，既有公用部分，但是又可以独立运行。那么对于用不到的我就不去打包他，这样可以大大减小生产包的size。

>理解服务分包部署这个模式，去模仿。注意的是，项目结构上要提前做到公用部分抽离，单个系统可以独立运行。

好了开始动手

```js
    /**
     * 说一下思路吧，之前一直说搞的，没搞： 🐕,ԾㅂԾ,,
     * 1. process.argv 获取 npm 命令上约定的参数值， eg: "npm run server -- system=a,b,c"
     * 2. 拿到system中的值，转换成数组，直接便利数组，组装pages就可以了
    */
```

![获取npm命令上携带的参数](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/vue/multi-page-6.png)

修改`package.json`

```json
    "scripts": {
        "serve": "vue-cli-service serve",
        "serve:module": "vue-cli-service serve server:module",
        "build": "vue-cli-service build",
        "build:module": "vue-cli-service build build:module",
        "lint": "vue-cli-service lint"
    },
```
修改`vue.config.js`
```js
const path = require('path')
const fs = require('fs')
const CopyWebpackPlugin = require('copy-webpack-plugin')
const { argv } = require('process')

const resolve = dir => path.resolve(__dirname, dir)

const isModuleServe = argv.findIndex(item => (item === 'server:module' ||  item === 'build:module')) !== -1;
const whiteModuleList = ['index']

const config = {
    entry: 'main.js',
    html: 'index.html',
    pagesRoot: resolve('src/pages')
}

const getSubSystem = () => {
    const allSubSystem = []
    const findAllSubSystem = (source, allSubSystem) => {
        const files = fs.readdirSync(source)
        for (let i = 0; i < files.length; i++) {
            const filename = files[i]
            const fullname = path.join(source, filename)
            const stats = fs.statSync(source, filename)
            if (!stats.isDirectory()) continue
            if (fs.existsSync(`${fullname}/${config.entry}`)) {
                /* 是否存在模块化 */
                if(isModuleServe) {
                    const pass = argv.find(item => item === filename) !== undefined || whiteModuleList.indexOf(filename) !== -1
                    /* 不在白名单且没有自定义打包的模块直接跳过， 结束本次for循环 */
                    /* indexOf为了去重，防止同一模块二次打包 */
                    if(!pass || (allSubSystem.length > 0 && allSubSystem.indexOf(filename) !== -1)) continue;
                    allSubSystem.push(fullname)
                } else {
                    allSubSystem.push(fullname)
                }
            }
        }
    }
    findAllSubSystem(config.pagesRoot, allSubSystem)
    console.log(allSubSystem)
    return allSubSystem
}

const getPages = () => {
    const pages = {}
    getSubSystem().forEach(page => {
        const filename = page.slice(config.pagesRoot.length + 1)
        pages[filename] = {
            entry: `${page}/${config.entry}`,
            template: `${page}/${config.html}`,
            filename: filename === 'index' ? config.html : `${filename}/${config.html}`
        }
    })
    return pages
}

const pages = getPages()

/* vue 配置 */
module.exports = {
    /* 省略其他代码 */
    pages,
    
}
```

模块启动 `yarn server:module finance` 或 `npm run server:module finance`

这样的话就可以做到用到哪个子系统，打包和启动哪个。