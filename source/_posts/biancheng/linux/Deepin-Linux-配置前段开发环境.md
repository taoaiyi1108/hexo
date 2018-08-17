---
title: Deepin(Linux)配置前段开发环境
date: 2018-08-17 14:46:40
tags: 
    - Deepin
    - Linux
categories: 编程
---
<h4>让初学者体验Linux系统飞一般的感觉，Mac的感觉</h4>

<!-- more -->
##### Deepin 前端环境搭建，[原文链接](https://blog.csdn.net/fungleo/article/details/78434813)
- 安装node.js
    - 首先，我们打开 nodejs 官方网站 https://nodejs.org/en/ 点击菜单栏的 Download 链接，进入下载界面
    <div align='center'>![](http://p040q6o73.bkt.clouddn.com/deepin-web-1.png)</div>
    - 滚动页面到下面，点击 Installing Node.js via package manager 链接，进入用包管理安装软件的页面。
    <div align='center'>![](http://p040q6o73.bkt.clouddn.com/image/deepin-web-2.png)</div>
    - 点击 `Debian and Ubuntu based Linux distributions` 跳转到安装指导内容区域；
    <div align='center'>![](http://p040q6o73.bkt.clouddn.com/deepin-web-3.png)</div>
    - 我们可以看到，执行命令 `sudo apt-get install -y nodejs` 来进行安装 nodejs，然后我们就打开终端，输入这个命令，然后盲输入密码，就可以安装我们需要的 nodejs 了。
    - 安装完成后在终端中输入 `node -v` 就可以查看node版本了； `v6.1.3` 

- 安装 npm 包管理器
    - `sudo apt-get install -y npm`
    - 安装完成后，终端输入`npm -v` 有版本 输出 说明npm 已经安装完成

- 安装 git 版本工具
    - `apt-cache search git | grep ^git`
    - `sudo apt-get install git -y`
    - `git --version `到输出了正确的 git 版本号。说明我们的 git 已经安装完成了。

- 安装 vue 
    - `sudo npm install vue-cli -g`

- 安装 react 
    - `sudo npm install create-react-app -g`

- 安装 react-native Yarn
    - `sudo npm install -g yarn react-native-cli`  

##### Node.js 版本过低 升级, [原文链接1](https://www.jianshu.com/p/b61f7988688d), [原文链接2](https://github.com/nodesource/distributions/issues/442)
- 按照上面流程配置好 node 的版本是 6.1.3 但是在构建React-Native 项目的时候报错了`error react-native@0.56.0: The engine "node" is incompatible with this module. Expected version ">=8".` 意思说你的node版本要在8以上，那么问题来了，node如何升级；
- 升级方法
- `wget https://deb.nodesource.com/setup_8.x -O node_8.x.sh` 下载来基本文件
- 终端执行 `vi node_8.x.sh`(修改文件，也可以右键用Text文本编译器打开修改) 
- 找到DISTRO=$(lsb_release -c -s)这行，修改为：DISTRO="jessie"
- 将`node_8.x.sh` 移动到 主目录 Downloads（下载）文件下改名为`script.sh`
- 终端执行 `cat ./Downloads/script.sh | sudo -E bash -`
- 终端执行 `sudo apt update && sudo apt install nodejs`
- 安装完成后 执行 `node -v` 输出 `v-8.11.4`
- node 升级成功