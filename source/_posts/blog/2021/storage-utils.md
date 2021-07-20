---
title: Storage 工具类
date: 2021-1-5 10:17
tags: 'storage'
categories: '2021'
---
简易封装Storage本地缓存工具类
<!-- more -->

一个简单的带有过期时间本地缓存方法脚本，封装的不好不会要见笑
```js
/**
 * 封装 web端缓存
 * @Base64 简单的加密下，避免明文
 * @key
 * @value 
 * @maxAge 自定义过期时间
 * @maxAgeTime 默认过期时间
 */
import { Base64 } from 'js-base64'

class Session {
    static maxAgeTime = new Date().getTime() + 1000 * 3600 * 8; // session 过期时间

    static setLocalSession(key, value, maxAge) {
        try {
            const data = { value,  maxAge: maxAge || this.maxAgeTime }
            localStorage.setItem(typeof key === 'string' ? key : JSON.stringify(key), Base64.encode(JSON.stringify(data)))
        } catch (error) {
            console.warn(error)
        }
    }

    static getLocalSession(key) {
        try {
            const date = new Date().getTime()
            const localdata = localStorage.getItem(typeof key === 'string' ? key : JSON.stringify(key));
            if(!localdata) return
            const sessionData = JSON.parse(Base64.decode(localdata))
            if (sessionData  && sessionData.maxAge != null && sessionData.maxAge - date > 0 && sessionData.maxAge <= this.maxAgeTime ) {
                this.setLocalSession(key, sessionData.value)
                return sessionData.value
            } else {
                return null
            }
        } catch (error) {
            console.warn(error)
        }
    }

    static removeSession(key) {
        if(Array.isArray(key)) {
            key.forEach(item => {
                localStorage.removeItem(typeof item === 'string' ? item : JSON.stringify(item))
            })
        } else {
            localStorage.removeItem(typeof key === 'string' ? key : JSON.stringify(key))
        }
    }

    static removeAllSession() {
        localStorage.clear()
    }
}
export default  Session
```