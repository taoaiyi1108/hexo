---
title: WebSocket 工具类
date: 2021-1-6 08-57
tags: 'websocket'
categories: '2021'
---
简易封装WebSocket工具类
<!-- more -->

封装的不好多多包涵
```js
class WebsocketServer {
    
    constructor(socket_name, ws_url, params) {
        /**
         * @socket_name socket名称
         * @ws_url  socket服务器地址
         * @params 要传递的参数
         */
        this.message = 'message'
        this.ws = window._websocket ? window._websocket[socket_name] : null;
        // 判断当前浏览器是否支持WebSocket
        if ('WebSocket' in window) {
            let ws = this.ws
            if (ws && ws.readyState === 1) {
                // socket 已存在 且已建立连接
                ws.send(this.message)
            } else if (ws && ws.readyState === 0) {
                // socket已存在，正在连接中
                setTimeout(() => {
                    ws.send(this.message)
                }, 1000)
            } else {
                // socket 还没有创建
                const paramsStr = this.serialization(ws_url, params)
                ws = new WebSocket(paramsStr)
                window._websocket = window._websocket ? Object.assign(window._websocket, { [socket_name]: ws }) : Object.assign({}, { [socket_name]: ws })

                ws.onopen = () => {
                    // Web Socket 已连接上，使用 send() 方法发送数据
                    ws.send(this.message);
                };
    
                ws.onmessage = function (evt) {
                    // socket 通信时 要处理的业务逻辑
                };
    
                ws.onclose = () => {
                    // 关闭 websocket
                    ws.close();
                    delete window._websocket[socket_name]
                    this.ws = null
                };
    
                ws.error = function () {
                    //socket 报错时
                }
            }
        } else {
            alert('您的浏览器不支持 WebSocket!')
        }
    }
    // 序列化参数
    serialization(ws_url, params) {
        let str = ''
        if(params && params instanceof Object) {
            for (const key in params) {
                if (params.hasOwnProperty(key)) {
                    const item = params[key];
                    str = str + `${key}=${item}&`
                }
            }
            return `${ws_url}?${str}`
        } else {
            return ws_url
        }
        
    }
    // 销毁关闭
    static destroyed(socket_name) {
        const ws = window._websocket ? window._websocket[socket_name] : null;
        if (ws && ws.readyState === 1) {
            // socket 已存在时销毁关闭
            ws.close()
        }
    }
}
```