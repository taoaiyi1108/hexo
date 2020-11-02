---
title: Springboot 构建http服务，返回的http行是 HTTP/1.1 200
date: 2020-09-26 18:03:16
tags: Springboot nuxt
categories: 编程
---
nuxt开发IE http 异常

<!-- more -->

    公司需要做一个信息类网站， 就是使用nuxtjs来解决seo问题，兴高采烈的做完结果，IE爆了，直接上图

![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/server/serve-nuxt-1.png)

    问题描述(红色框)中的api直接请求的协议是HTTP1.1 返回状态200OK, 返回值无；(绿色框)中的api是正常的，

    吐血搜所后找到问题 [连接](https://blog.csdn.net/weixin_30378311/article/details/97312282) 

    再次感谢这位兄台，

    问题点： Springboot 版本问题，项目用的是2.2.5的版本，按照老哥的降低版本值1.5.9，问题解决

    后看官网1.5.9使用下载了最多， 看来新的的东西还是别轻易尝试


![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/server/serve-2.png)
