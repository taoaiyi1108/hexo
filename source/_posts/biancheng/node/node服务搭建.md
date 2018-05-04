---
title: node服务搭建
date: 2018-05-02 15:00:22
tags: node
categories: 编程
---
#### 使用express搭建node服务

<!-- more -->
##### 安装express
- 全局安装 `npm install -g express-generator@4`

##### 创建服务
- 创建你需要搭建service的文件夹
- 创建服务 `express -e name`  基于ejs接近html格式容易看得懂
- `cd name` & ` npm install`
- `npm start` 启动服务
- 浏览器打开服务 `localhost:3000` 默认端口3000
- 配置文件 `./bin/www`
- app.js 是服务入口文件

##### 热更新插件
- `npm install -g supervisor`
- 启动项目命令`supervisor bin/www`

##### 目录结构
- app.js 入口文件
- views 视图展示（页面）
- routes 路由
    + index.js

##### 输出结构
- 页面输出（默认）
    + views/index.ejs(类似于html结构)
    + routes/index.js里面的代码
    ```javascript
    var express = require('express');
    var router = express.Router();
    /* GET home page. */
    router.get('/', function(req, res, next) {
        res.render('index', { title: 'Express' });
    });
    module.exports = router;
    ```
    + 输出效果图
    ![](http://p040q6o73.bkt.clouddn.com/image/node/node-express-1.png)
- json对象输出
    + 修改route/index.js里面的代码
    ```javascript
    var express = require('express');
    var router = express.Router();
    /* GET home page. */
    router.get('/', function(req, res, next) {
        /*res.render('index', { title: 'Express' });*/
        return res.send({
            status:1,
            info:'美女服务'
        })
    });
    module.exports = router;
    ```
    + 输出效果图
    ![](http://p040q6o73.bkt.clouddn.com/image/node/node-express-2.png)

---
#### 实战项目走起


##### 开发node service完成一个项目的前提
- 服务如何去架构？数据库选择？使用哪种orm模型？是否需要安全认证？ 用户机制
- 举例：
    ![](http://p040q6o73.bkt.clouddn.com/image/node/node-express-3.png)

##### 接口设计
- 前四个接口有个共同特征就是url path是一致的，后面的参数不一致，参数代表的是一个一个对应json的文件名，运行一段js，读取参数返回对应json文件数据
    ![](http://p040q6o73.bkt.clouddn.com/image/node/node-express-4.png)

##### 读取数据模块
- 在改成的public文件夹下新建dataw文件夹存放json数据文件
    + it.json 注意新建的json文件中即便是空也必须有一个 `[]` ,否则会因为获取到的数据是非json格式而报错
- data文件夹下新建data.js,我们已知node开发一个服务，只需要在route/index.js中return res.sendd返回一个json就可以了
- 拷贝route/index.js中的代码到data.js中
    ![](http://p040q6o73.bkt.clouddn.com/image/node/node-express-5.png)
- 修改入口文件app.js中的路由设置
    - 添加我们的路由 `var var data = require('./routes/data');`
    - 使用 `app.use('/data', data);`
    ![](http://p040q6o73.bkt.clouddn.com/image/node/node-express-6.png)
- 修改data.js 构造j接口 `data/read?type=it`
```javascript
/* data.js*/
var express = require('express');
var router = express.Router();

/*引入node读取文件模块*/
var fs = require('fs')
/*引入json数据*/
var PATH = './public/data/'

/* 读取数据模块 */
// data/read?type=it   data/read?type=it.json  接口例子
router.get('/read', function(req, res, next) {
  var type = req.param('type') || '';
  fs.readFile(PATH + type + '.json',(err,data) =>{
    // 回调函数 es6写法 
    // fs.readFile(PATH + type + '.json',function(err,data){}) es5写法
    if(err){
        return res.send({
            status:0,
            info:'读取文件失败'
        })
    }
    var COUNT = 50; //一次返回50条数据
    //TODO:try 对非json字符串的数据解析 让代码继续运行 
    var obj = [];
    try{
        obj = JSON.parse(data.toString());
    }catch(e){
        obj = []
    }
    if(obj.length > COUNT){
        obj = obj.slice(0,COUNT)
    }
    return res.send({
        status:1,
        data:obj
    })
  })
});

module.exports = router;
```
- 在浏览器中打开`http://localhost:3000/data/read?type=it`
```javascript
    //报错 http://localhost:3000/data/read?type=itt 本来就没有该文件
    {
        "status": 0,
        "info": "文件读取失败"
    }
    //正常 http://localhost:3000/data/read?type=it
    {
        "status": 1,
        "data": []
    }
    //it.json中添加数据后返回的数据
    {
        "status": 1,
        "data": [
            {
                "title": "新闻",
                "url": "www.xxx.com",
                "img": "img.xxx.com/a.png"
            }
        ]
    }
```
- 对json数据进行try catch处理之后新建md.json,即便md.json中没有必要的`[]`,请求`http://localhost:3000/data/read?type=md`,也可以避免报错，返回的是一个空数组数据，让代码继续执行;
```javascript
{
    "status": 1,
    "data": []
}
```

##### 数据存储模块
- 继续修改data.js文件
    - guid生成的算法有很多[链接](https://www.cnblogs.com/snandy/p/3261754.html)
```javascript
/* data.js*/
var express = require('express');
var router = express.Router();

/*引入node读取文件模块*/
var fs = require('fs')
/*引入json数据*/
var PATH = './public/data/'

/* 读取数据模块 */
// data/read?type=it   data/read?type=it.json  接口例子
router.get('/read', function(req, res, next) {
  var type = req.param('type') || '';
  fs.readFile(PATH + type + '.json',(err,data) =>{
    // 回调函数 es6写法 
    // fs.readFile(PATH + type + '.json',function(err,data){}) es5写法
    if(err){
        return res.send({
            status:0,
            info:'读取文件失败'
        })
    }
    var COUNT = 50; //一次返回50条数据
    //TODO:try 对非json字符串的数据解析 让代码继续运行 
    var obj = [];
    try{
        obj = JSON.parse(data.toString());
    }catch(e){
        obj = []
    }
    if(obj.length > COUNT){
        obj = obj.slice(0,COUNT)
    }
    return res.send({
        status:1,
        data:obj
    })
  })
});
/* 数据存储模块 */

//一般数据写入我们post 将 router.get 改为 router.post 即可
router.post('/write',function(req,res,next){
    var type = req.param('type') || ''; //获取需要写入数据的文件
    // 获取关键点的字段
    var url = req.param('url') || '';
    var title = req.param('title') || '';
    var img = req.param('img') || ''; //我们不上传图片，只存储图片的地址，节省空间，图片可以放在七牛云

    //预处理避免传入的字段空字符创
    if(!type || !title || !url || !img){
        return res.send({
            status:0,
            info:'提交的字段不全'
        })
    }
    // 1)读取文件
    var filePath = PATH + type + '.json'; //提取路径优化代码
    fs.readFile(filePath, function(err,data){
        if(err){
            res.send({
                status:0,
                info:'读取数据失败'
            })
        }
        var arr = JSON.parse(data.toString());
        //代表每一条记录
        var obj = {
            img:img,
            url:url,
            title:title,
            id:guid(),
            time:new Date()
        }
        // 将数据插入到数组的最前面
        arr.splice(0,0,obj)
        // 2)写入文件
        var newData = JSON.stringify(arr);
        fs.writeFile(filePath, newData, function(err){
            if(err){
                return res.send({
                    status:0,
                    info:'写入文件失败'
                })
            }
            // 成功返回当前的记录
            return res.send({
                status:1,
                data:obj
            })
        })
    })
})

/* guid */
function guid() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        var r = Math.random()*16|0, v = c == 'x' ? r : (r&0x3|0x8);
        return v.toString(16);
    });
}


module.exports = router;
```
- 我们之前新建的md.json中没有任何数据，现在开始写入
    + 浏览器输入`http://localhost:3000/data/write?type=md&url=xxx.com&img=xxx.com/img&title=写入的数据`
    + 返回的数据
    ```javascript
    {
        "status": 1,
        "data": {
            "img": "xxx.com/img",
            "url": "xxx.com",
            "title": "写入的数据",
            "id": "de24e984-b4f2-42a4-84fc-f70a45d2c3ab",
            "time": "2018-05-04T09:00:02.112Z"
        }
    }
    ```
    + 回头查看md.json文件，也有了我们写入的数据
    ![](http://p040q6o73.bkt.clouddn.com/image/node/node-express-7.png)