---
title: 聊聊用React去搭建一个CMS项目的流程
date: 2021-03-24 22:36:52
tags: element
categories: 编程
---
回顾一下当时的艰辛，苦逼的板砖

<!-- more -->

### 说在开头 

项目是20年7月份做的，不大，也是第一次去用React做项目，有些许收获，现在重新梳理一下，后面以备不时之需，毕竟日常中接触的大多是Vue项目，担心生疏了。
有不对的地方还望各位同学见谅！

- [源码](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/react/react-admin.zip)
- [线上地址](http://47.97.48.155:8080/) 有点卡，耐心点 admin 123456



### 准备

`npx create-react-app my-app` 先构建一个空白项目

### 修改配置文件，定制化项目

如果你需要深度定制化项目的话，建议你暴露React配置文件，执行`npm run eject`, 删除`node_modules`, 在重新`npm install`

>需要注意的是 `npm run eject` 这个过程是不可逆的， 踩坑感觉这个过程需要在你项目构建后未安装任何其他的依赖前执行，否则会执行失败，所以说这是一个二叉路口，需要你提前走位预判

当然了，修改React配置文件，定制化项目还有其他路径，借助第三方库 `react-app-rewired` 配合 `customize-cra`, 推荐

#### react-app-rewired  customize-cra

安装：`npm install react-app-rewired customize-cra -S`
在项目跟目录下出创建一个`config-overrides.js`, 有点类似与`Vue-cli3.x`中的`vue-config.js`

```js
//config-overrides.js
const path = require('path')
const { override, fixBabelImports, addWebpackModuleRule, addWebpackAlias, addBabelPlugins  } = require('customize-cra')

const pathResolve = pathUrl => {
    return path.join(__dirname, pathUrl)
}

export.modules = override(
    // 集成babel插件
    ...addBabelPlugins(
        [
            "styled-jsx/babel", // 可以在JSX中写css
        ]
    ),
    // 集成antd 的样式
    fixBabelImports('import', {  
        libraryName: 'antd',
        libraryDirectory: 'es',
        style: true
    }),
    addWebpackAlias({
        "@": pathResolve('./src'), // 配置路径别名
    }),
    // 配置 webpack的rule， 我这里集成了less, 全局使用less变量
    addWebpackModuleRule(
        // 全局使用less变量
        {
            test: /\.less$/,
            exclude: /\.module\.less$/,
            use: [
                'style-loader',
                'css-loader',
                {
                    loader: 'less-loader', // compiles Less to CSS
                    options: {
                        lessOptions: { // 如果使用less-loader@5，请移除 lessOptions 这一级直接配置选项。
                            // 自定义 Antd的主题色
                            modifyVars: {
                                'primary-color': '#37A2DA',
                                'border-radius-base': '2px',
                                'font-size-base': '13px',
                            },
                            javascriptEnabled: true,
                        },
                    }
                },
                {
                    loader: 'style-resources-loader',
                    options: {
                        patterns: [
                            pathResolve('./src/style/theme.less'),
                            pathResolve('./src/style/common.less'),
                        ]
                    }
                },
            ]
        }
    )
)
```

`package.json`中还要去修改`scripts`

```json
"scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
},
```

>当然我写的很谁的你看一看这个同学的 ！[react-app-rewired使用](http://wmm66.com/index/article/detail/id/165.html)， 或者 [官方中文Docs](https://github.com/timarney/react-app-rewired/blob/HEAD/README_zh.md)

### 选取集成UI库

`React`项目一般就是 [Ant Design](https://ant.design/index-cn) ,  `Vue`项目一般就是 [element-ui](https://element.eleme.cn/#/zh-CN)
集成过程可以看[官方Docs](https://ant.design/docs/react/use-with-create-react-app-cn)

需要注意的是 `Ant Design` 直接使用，个别组件（TimePicker）是 `english`，需要 [全局配置](https://ant.design/components/config-provider-cn/#header)

```jsx
// App.jsx
import { ConfigProvider } from 'antd';
import zhCN from 'antd/es/locale/zh_CN';

export default () => (
  <ConfigProvider locale="{zhCN}">
    <App />
  </ConfigProvider>
);
```

### 集成Store Redux

一个CMS项目，`Store`怎么能少呢

`npm install react-redux redux redux-thunk -D` 

```js
/**
 * App.js
*/
import { Provider } from 'react-redux'
import store from './store'
export default function App() {
    return (
        <Provider store={store}>
            <OtherComponents />
        </Provider>
    )
}

```

```js
/**
 * store
 * /store/index.js
*/
import { createStore, applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk'
import reducers from './reducers'

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose

const store = createStore(
    reducers,
    // dev环境下使用Redux DevTools, React需要自己去配置，，Vue的话已经内部集成好了
    process.env.NODE_ENV === 'development' ? composeEnhancers(applyMiddleware(thunk)) : applyMiddleware(thunk) 
)
export default store
```

```js
/**
 * reducers
 * /store/reducers.js
*/
import { combineReducers } from 'redux'  

import login from './modules/loginReducers'
import user from './modules/userReducers'

// 多个reducer合并
export default combineReducers({
    login,
    user
})
```

```js
/**
 * userReducers
 * /store/userReducers.js
*/

import UserApi from '@/api/user-api'
const initialState = {
    userinfo: {},
    buttonIds: [],
    menuIds: [],
    menuTree: []
}

const types = {
    GET_USERINFO: 'USER/GET_USERINFO',
    GET_PROMISSION: 'USER/GET_PROMISSION'
}
const getUserInfo = userinfo => ({ type: types.GET_USERINFO, userinfo })
const getPromission = promisson => ({ type: types.GET_PROMISSION, promisson })

export const actions = {
    /* 获取用户信息 */
    getUserInfo() {
        return (dispatch, getState) => {
            const { user } = getState()
            if (!user.userInfo) {
                UserApi.getUserinfo().then(res => {
                    dispatch(getUserInfo(res.data))
                })
            }
        }
    },
    /* 获取用户权限 */
    getUserPromission() {
        return (dispatch, getState) => {
            UserApi.getUserPrivilege().then(res => {
                const { data: { buttonIds, menuIds, menuTree } } = res
                dispatch(getPromission({ buttonIds, menuIds, menuTree }))
            })
        }
    }
}

export default (state = initialState, action) => {
    switch (action.type) {
        case types.GET_USERINFO:
            return { ...state, userinfo: { ...action.userinfo } }
        case types.GET_PROMISSION:
            return { ...state, ...action.promisson }
        default:
            return state
    }
}
```

在`userReducers`中我把 `initialState, actions` 和 `reducer` 写在了一起， 其实可以和`Vuex`的类似，分开来去写。

对比`Vuex`来理解理解， `initialState <---> state`，`reducer <---> commit`， `actions <---> actions`

`initialState`初始化数据，使用`dispatch`派发`action`，在`reducer`中判断`action`的`type`, 来对不同数据进行`reducer`修改，需要注意的是：
    1. `redux-thunk` 将我们的在 `action`中的一步http转换为同步操作，
    2. `react-redux`中的 `connect`可以把`state`的值和`action`中的方法放到组件的`props`中 便于使用


### 集成Router， 也是项目的核心点

CMS类型的项目， 主干流程 `用户登录 --> 权限判断 --> 展示数据`, 看起来很简单， 做起来考虑的事情可就多了，😀 

`npm install react-router-dom -S`

集成进React
```js
// App.js
import { Provider } from 'react-redux'
import { BrowserRouter, Switch, Route } from 'react-router-dom'
import store from './store'
import AuthRouter from './auth-router' // 权限验证的一个组件
import Login from '../../views/login' // 登录组件
import Layout from '../../components/layout' // CMS的Layout组件
export default function App() {
    return (
        <Provider store={store}>
            <BrowserRouter>
                <Switch>
                    <Route component={Login} exact path="/login" />
                    <AuthRouter path="/" component={Layout} />
                </Switch>
            </BrowserRouter>
        </Provider>
    )
}
```

```js
//AuthRouter
import React from 'react'
import { connect } from 'react-redux'
import { withRouter, Route, Redirect } from 'react-router-dom'

const AuthRouter = ({ component: Component, login, ...rest }) => {
    return <Route {...rest} render={
        props => ( 
            login ? 
            <Component {...props} /> : 
            <Redirect to={'/login'} /> 
        )} />;
}

export default connect(state => ({ login: state.login.login }), null)(withRouter(AuthRouter))

```

```js
// Layout 在登录权限之后，每一个page还要根据server端返回的权限做校验
import React from 'react'
import { connect } from 'react-redux'
import { Switch, Route, Redirect, withRouter } from 'react-router-dom'
import { routes } from '@/container/router/routes'

/* 校验二级菜单 */
const AuthRouter = ({ component: Component, role, ...rest }) => {
    return <Route {...rest} render={props => (role && <Component {...props} />)} />;
}

function Container({ location, hideBreadcrumb }) {
    return (
        <div className={`content-container-content ${!hideBreadcrumb ? 'hide-breadcrumb' : ''} `}>
            <Switch location={location}>  {/* location={location} 这个一定要配置 否则会有重复请求api的问题 */}
                {
                    routes.map( route => (
                        <AuthRouter path={route.path} role={route.role} meta={route.meta} key={route.path} component={route.component} />
                    ))
                }
                <Redirect from='/' exact to='/dashboard' />
                <Redirect to="/errorpage/404" />
            </Switch>
        </div>
    )
}

export default connect(state => ({ hideBreadcrumb: state.breadcrumb.show }))(withRouter(Container))
```

```js
//routes 类似Vue-Router 集中处理
import asyncImportComponent from './async-import-component'

export const routes = [
    {
        path: "/dashboard",
        meta: { name: '首页', icon: 'dashbord' },
        role: 'dashboard',
        component: asyncImportComponent(() => import('../../views/dashboard'))
    },
    {
        path: "/news",
        meta: { name: '新闻信息', icon: 'xinwen' },
        role: 'news',
        component: asyncImportComponent(() => import('../../views/news'))
    }
]
```

```js
// asyncImportComponent  异步加载组件
import React, { Component } from 'react'

export default (importComponent) => {
    return class asyncComponent extends Component {
        state = { component: null }

        componentDidMount() {
            importComponent().then(mod => {
                this.setState({ component: mod.default })
            })
        }

        render() {
            const C = this.state.component
            return C ? <C {...this.props} /> : null
        }
    }
}
```


### 写在最后

上述呢只是我的项目流程，每一个同学有自己的理解，每一个项目有自己的独特之处，Author验证上需要前后端配合的，在这里我只是提供一个思路而已，但我也觉得我应该还是`Vue`的那一套思路，没有体会到`React`的精髓。

最后说点自己的感想：

    1. `React`对比`Vue`确实难啃
    2. `Class Component` 和 `Function Component` 理解起来容易，实际开发的时候就没必要矫正了
    3. `Hooks`确实好用


想了解 `Hooks`的同学可以看这里[React Hooks 详解](https://juejin.cn/post/6844903985338400782)