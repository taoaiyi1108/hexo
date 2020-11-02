---
title: nginx 部署服务
date: 2020-09-26 16:26:55
tags: nginx
categories: 编程
---
nginx 在项目部署过程中的操作
<!-- more -->

#### nginx 部署Vue多页面

```
    server {
        listen       9528; # 服务启动的端口号  http://192.168.0.168:9527
        server_name  localhost;

        # http://192.168.0.168:9527
        location / {
			root /html; # 页面所在根路径路径 注意文件夹在盘符的根目录下
			index index.html;
			try_files $uri /index.html; # 对应页面所作路径
		}

        # http://192.168.0.168:9527/system
		location /system {
			root /html;
			index index.html;
			try_files $uri /system/index.html;
		}
	}

```

#### nginx 部署Nuxt项目

- `nuxt buidl`打包
- 复制 `.nuxt` `nuxt.config.js` `static` `node_modules`（可以复制也可以重新`install`）到服务器上去
- 当前文件夹下创建`startscript.js`
    ```js
        var cmd=require('node-cmd'); cmd.run('npm start');
    ```
- 一定要记着 `npm install node-cmd ` 不然会报错
- `npm install pm2 -g` 服务器上安装 `pm2`

- `pm2 start startscript.js` 启动成功

![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/server/server-pm2-1.png)

```js

    # pm2 list 查看所有PM2 服务
    # pm2 start 启动所有服务
    # pm2 start 0 启动对应服务
    # pm2 stop 停止所有服务
    # pm2 stop 0 停止对应服务
    # pm2 delete 删除所有服务
    # pm2 delete 0 删除对应服务

```


- `nginx`配置代理

注意： nuxt start 不可以用 ip直接访问(可能我不会设置吧)

```
    # http://192.168.0.168:9527
    
    server {
        listen       9527;
        server_name  localhost;

        location / {
		   proxy_pass   http://127.0.0.1:3000;	# nuxt start 启动后的端口
		}
	}

```