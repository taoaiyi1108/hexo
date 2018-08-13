---
title: react-native 爬坑记录
date: 2018-08-11 14:23:02
tags: reactnative
categories: 编程
---
<h4>React-Native 开发爬坑之旅</h4>

<!-- more -->

 1.React-Native 启动报：`The development server returned response error code: 500 in react-native`;
- 解决方法：
    - 度娘上很多人都入到这个问题了，有人说 node 没有启动，有人说 modules 依赖没有安装好，npm 不行就用yarn...但问题都没有解决，奈何English终端面板报错又看不懂，只能一点点的阅读；最终找到一下较为靠谱的解决方法；
    - 1.啥都不说，链接走起 [移步这里](https://blog.csdn.net/qingsong_xu/article/details/72515075)；

    ```javascript
        1、watchman watch-del-all 
        2、rm -rf node_modules && npm install 
        3、rm -fr $TMPDIR/react-*
    ```
    但是我发现第一条命令就出问题了，`watchman 不是内部或外部命令` 而且我也不知道他是干嘛的，第二条知道，跑完命令，启动项目没卵用，GG，第三条算了，好吧 这个错误和我的遇到的很接近，但方法不适合我，废弃；
    - 如果对你有用请笑纳！
    - 2.啥都不说，链接走起 [移步这里](https://blog.csdn.net/qq_28484355/article/details/81288810)；
    后来我查看 node server 中提到了`AccessibilityInfo`这个单词，好吧我就去找你的原因，果然就有人也遇到了这个问题；
        - 问题:RN工程运行报错`Unable to resolve module 'AccessibilityInfo'...doesn't exist in the Haste module map`,
        - 这个问题似乎是由于0.56.0版本所引起的依赖关系的错误，打开根目录下的package.json，查看react-native的版本是不是0.56.0
        - 若是0.56.0，则将其降级为0.55.4，在你项目的根目录下打开命令行（或者cd到项目根目录下）
        - 运行：`npm install react-native@0.55.4`
    - 降级 react-native 完成之后 运行 `react-native run-anroid` 久违的画面` Wellcome React-Native`


2.ReactNative 程序调试时报 `unable to connect with remote debugger`
- 解决方法：啥都不说,链接走起[移步这里](https://stackoverflow.com/questions/40898934/unable-to-connect-with-remote-debugger)
    - 默认Chrome打开的url `http://10.0.2.2:8081/debugger-ui`
    - 替换成`http://localhost:8081/debugger-ui`
    - 或者点击DEV Settings ->Debug server host&port for device 配置电脑的ip:8081。[举例](https://www.yeshen.com/blog/reactnativeyeshen/)