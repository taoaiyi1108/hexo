---
title: 个人对react-hooks的理解
date: 2021-4-6 00:56
tags: 'react'
categories: '2021'
---
学以致用吧，总结归纳

<!-- more -->


### 写在开头 [源码](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/react/react-hooks.zip)

- 自己主要还是以`vue`开发为主，`react`项目也是做过一些，主要是以`class`组件的模式在开发，`react hooks`推出后虽说用过，也是写简单的使用，现阶段官方也是提倡使用`hooks`模式开发，自然有他的道理，我个人最近也是琢磨了许久，个人总结一下，后期复习用；
- 自己才疏学浅在这里只是谈谈`react hooks`的使用场景，源码之类的就不详解了，有兴趣的同学找更牛逼的`blog`看。
- [官方Docs](https://react.docschina.org/docs/hooks-intro.html)
- 说的不对的地方还望见谅

### Hooks解决的问题

#### 1、类组件的不足
- 状态逻辑难复用： 在组件之间复用状态逻辑很难，可能要用到 `render props` （渲染属性）或者 `HOC`（高阶组件），但无论是渲染属性，还是高阶组件，都会在原先的组件外包裹一层父容器（一般都是 div 元素），导致层级冗余
- 趋向复杂难以维护：
    - 在生命周期函数中混杂不相干的逻辑（如：在 `componentDidMount` 中注册事件以及其他的逻辑，在 `componentWillUnmount` 中卸载事件，这样分散不集中的写法，很容易写出 bug ）
    - 类组件中到处都是对状态的访问和处理，导致组件难以拆分成更小的组件
- this 指向问题：父组件给子组件传递函数时，必须绑定 `this`

```js
    /* this绑定的几种方法 */
    Class App extends React.Component {
        handleClick;
        constructor(props) {
            super(props);
            this.state = {
                num: 1,
                title: "Tom"
            }
            this.handleClick = this._handleClice1.bind(this) // 第一种
        } 

        _handleClick1() {
            this.setState({ num: this.state.num + 1 })
        }

        _handleClick2 = () =>{
            this.setState({ num: this.state.num +1 })
        }

        render() {
            const { title } = this.state
            return(
                <div>
                    <h2>hello, {title}</h2>
                    <button onClick={this.handleClick}>btn1</button>
                    <button onClick={this._handleClick1.bind(this)}>btn2</button> {/* 第二种 */}
                    <button onClick={() => this._handleClick1()}>btn3</button> {/* 第三种 */}
                    <button onClick={this._handleClick2}>btn4</button> {/* 第四种 */}
                </div>
            )
        }
    }
```

#### 2、Hooks的优势
- 可以解决上边提到的类组件的问题
- 让组件变得相对的灵活，复用性强
- 组件内部拆分更加简单
- 对于组件中的副作用处理起来更加方便

### Hooks的使用注意事项
- 只能在函数内部的最外层调用 Hook，不要在循环、条件判断或者子函数中调用
- 只能在 React 的函数组件中调用 Hook，不要在其他 JavaScript 函数中调用
- [官方给出的建议](https://react.docschina.org/docs/hooks-rules.html)

### useState
- 简单的来说他和 `this.state`的功能是一样的，初始化一些组件内部的数据状态
- 它传入一个初始值，每次函数执行都能拿到新值。 需要注意的是，通过 useState 得到的状态 name，在组件中的表现为一个常量，每一次通过 setName 进行修改后，又重新通过 useState 获取到一个新的常量。

```js
    /* class 组件 */
    import React from 'react'
    Class UseState extends React.Component {
        constructor(props) {
            super(props)
            this.state = {
                name: 'Tom',
                age: 12,
                work: { name: '外卖员', time: '2020-12-12' }
            }
        }

        changeState = () => {
            this.setState({
                age: 13
            })
        }

        render() {
            return(<button onClick={this.changeState}>btn1</button>)
        }
    }
```
```jsx
    /* hooks 组件 */
    import React, { useState } from 'react'
    const UseState = () => {
        const [name, setName] = useState('Tom')
        const [age, setAge] = useState(12)
        const [work, setWork] = useState({ name: '外卖员', time: '2020-12-12' })

        const onClick = () => {
            setWork({...work, name: '修理工'}) // setWork 是异步更新work的
            console.log(work) // 那么此时的work并非最新的work
        }

        return(<>
            <button onClick={() => setName('Jim')}>btn1</button>
            <button onClick={() => setAge(13)}>btn2</button>
            <button onClick={() => onClick()}>btn3</button>
        </>)
    }

    /* useState 数据更新注意点 */
    const Example = () => {
        const [count, setCount] = useState(0);
        const addClick = () => {
            /* 
                setCount(count++); 
                error 不可以这么写: 类似于 setCount(count = count + 1); 
                修改state只能通过setXXX的方法修改，这样的写法是直接去修改state, 自然会报错
            */
            setCount(count + 1); // setCount 是异步更新count
            console.log(count); // 那么此时的count不是最新的值
        }
        const subClick = () => {
            setCount((count) => {
                // 这里的count是参数，不是state的count
                count--;
                console.log(count); // 此时的count 是最新的值
                return count;
            })
        }

        console.log(count); // 此时的count是最新的值

        return (
            <div className="App-header item-page">
                <div>
                    <button onClick={() => addClick()}>+</button>
                    <h1>{count}</h1>
                    <button onClick={() => subClick()}>-</button>
                </div>
            </div>
        )
    }
```

### useEffect
- 简单的来说他就是做了类组件中的 `componentDidMount`、`componentDidUpdate` 和 `componentWillUnmount`
- `useEffect` 是用于处理各种状态变化造成的副作用，也就是说只有在特定的时刻，才会执行的逻辑, 在 `hooks` 出来之后，函数组件中没有 `shouldComponentUpdate` 生命周期，我们无法通过判断前后状态来决定是否更新。
- `effect`副作用指的是： `ajax` 请求、访问原生`dom` 元素、本地持久化缓存、绑定/解绑事件、添加订阅、设置定时器、记录日志等
- `useEffect` 不再区分 `mount` `update` 两个状态，这意味着函数组件的每一次调用都会执行其内部的所有逻辑，那么会带来较大的性能损耗。

- 由于`useEffect`是异步的，是在真实的`DOM`构建之后去执行，而 `componentDidMount`、`componentDidUpdate` 是在真实`DOM`构建之前去执行的

```js
    /* 下面就以一个修改 页面title的例子作以说明 */

    /* 类组件 */
    import React from 'react'
    Class ChangeTitle extends React.Component {
        state = { count: 0 }

        componentDidMount() {
            this._changeTitle()
        }

        componentDidUpdate() {
            this._changeTitle()
        }

        componentWillUnmount() {
            document.title = `React App`
        }

        _add = () => this.setState({count: this.state.count + 1})

        _changeTitle = () => {
            document.title = `You clicked ${this.state.count} times`
        }

        render() {
            return (<>
                <p>You clicked {this.state.count} times</p>
                <button onClick={this._add}>btn</button>
            </>)
        }
    }
```
```js
    /* Hooks 组件 */
    import React, { useState, useEffect } from 'react';
    const ChangeTitle = () => {
        const  [count, setCount] = useState(0)

        useEffect(() => {
            // 由于`useEffect`是异步的，是在真实的`DOM`构建之后去执行  此时的 count是最新的值 
            document.title = `You clicked ${count} times`
            return function() {
                // count 二次修改的时候，先执行这里的逻辑
                // 组件销毁的时候也会执行这里的逻辑
                document.title = "React App"; 
            }
        }, [count])

        /* 
         useEffect: 是`componentDidMount`、`componentDidUpdate` 和 `componentWillUnmount`的组合
         1. 如果不给依赖项 [] 的时候会执行2次 `componentDidMount`、`componentDidUpdate` 
         2. 依赖项更新的时候，useEffect Hook 也会被执行
        */
    }
    return (<>
        <p>You clicked {count} times</p>
        <button onClick={() => setCount(count + 1)}>btn</button>
    </>)
```

### useMemo
- 是在`DOM`更新前触发的，简单的来说就是和`shouldComponentUpdate`的作用类似
- `useMemo` 主要用于渲染过程优化，两个参数依次是计算函数（通常是组件函数）和依赖状态列表，当依赖的状态发生改变时，才会触发计算函数的执行。如果没有指定依赖，则每一次渲染过程都会执行该计算函数
- 在`useMemo`中使用`setState`你会发现会产生死循环，并且会有警告，因为`useMemo`是在渲染中进行的，你在其中操作`DOM`后，又会导致触发`memo`
- 记住，传入 `useMemo` 的函数会在渲染期间执行。请不要在这个函数内部执行与渲染无关的操作，诸如副作用这类的操作属于 `useEffect` 的适用范畴，而不是 `useMemo`。比如网络请求等
- `useMemo` 中的逻辑在第一次渲染的时候会被执行，后面是根据依赖的变化而变化，局部渲染
- 记住父组件每一次的`update`都会出发子组件的重新渲染

```js
    import React, {useState, useMemo} from 'react'
    const UseMemo = () => {
        const [count1, setCount1] = useState(0)
        const [count2, setCount2] = useState(0)
        const [count3, setCount3] = useState(0)
        const [count4, setCount4] = useState(0)

        useMemoComponent1 = useMemo(() => {
            return <Time />
        }, [count1])

        useMemoComponent2 = useMemo(() => {
            return <div>使用useMemo局部渲染<Time /></div>
        }, [count2])

        return (
            <>
                <div>
                    <p>模块1</p>
                    <button onClick={() => setCount1(count1 - 1)}>-</button>
                    <span>count1: {count1}</span>
                    <button onClick={() => setCount1(count1 + 1)}>+</button>
                    {useMemoComponent1}
                </div>
                <div>
                    <p>模块2</p>
                    <button onClick={() => setCount2(count2 - 1)}>-</button>
                    <span>count2: {count2}</span>
                    <button onClick={() => setCount2(count2 + 1)}>+</button>
                    {useMemoComponent2}
                </div>
                <div>
                    <p>模块3</p>
                    <button onClick={() => setCount3(count3 - 1)}>-</button>
                    <span>count3: {count3}</span>
                    <button onClick={() => setCount3(count3 + 1)}>+</button>
                    < Time />
                    <button onClick={() => setCount4(count4 - 1)}>-</button>
                    <span>count4: {count4}</span>
                    <button onClick={() => setCount4(count4 + 1)}>+</button>
                </div>
            </>
        )
    }

    /* 子组件Time */
    const Time = () => (<p>{Date.now()}</p>)


    /**
     * 模块1和模块2中的 useMemoComponent1， useMemoComponent2使用了useMemo包装，只有在对应的依赖项变化的时候才会更新，
     * 而没有被useMemo包装的模块3，count3和count4改变的时候都会被触发渲染
    */
```

### useCallback
- 如果有函数传递给子组件，使用 `useCallback`， 配合`meno`使用。这句话的理解就是：一个父组件中有多个子组件，有一个子组件中的`button`要触发父组件中的方法，父组件更新就会导致他下面所有的子组件一起重新渲染，而原则上是哪个子组件触发父组件的方法，就应该只有他自己更新，其他的就不要动了，这样可以节省性能， `useCallback`就是来处理这个问题的，类似于`useMemo`的局部更新

```js
    import React, {useState, useCallback} from 'react'
    const UseCallback = () => {
        const [count1, setCount1] = useState(0)
        const [count2, setCount2] = useState(0)

        const clickButton1 = () => setCount1(count1 + 1)

        const clickButton2 = useCallback(() => {
            setCount2(count2 + 1)
        }, [count2])

        const clickButton3 = useCallback(() => {
            console.log('Button3')
            setCount3(count3 + 1)
        }, [count3])

        return(<>
            <div>
                <p>count1： {count1}</p>
                <Button onClickButton={clickButton1}>Button1</Button>
            </div>
            <div>
                <p>count2： {count2}</p>
                <Button onClickButton={clickButton2}>Button2</Button>
            </div>
            <div>
                <p>count3： {count3}</p>
                <Button onClickButton={clickButton3}>Button3</Button>
            </div>
        </>)
    }
    /* 子组件 */
    const Button = React.memo(({onClickButton, children}) => {
        return(<>
            <button onClick={onClickButton}>{children}</button>
            <span>{Math.random()}</span>
        </>)
    })

    /**
     * Button1点击的时候，Button2、Button3通过useCallback包装，指定依赖的是count2、 count3，就没有被重新渲染
     * Button2点击的时候只有他自己被重新渲染，Button1没有通过useCallback包装指定依赖，此时父组件update，导致Button1重新渲染
    */
```

### useRef
- `useRef` 返回一个可变的 `ref` 对象，其 `.current` 属性初始化为传递的参数（`initialValue`）。
- 访问`DOM`的主要方式
- 注意的是：`createRef` 每次渲染都会返回一个新的引用，而 `useRef` 每次都会返回相同的引用。

```js
    /* 一个input自动获取焦点的例子 */
    import React, { useRef, createRef, forwardRef } from 'react'
    // forwardRef 转发ref
    const ChildComp = forwardRef((props, inputRef) => {
        const onClick = () => {
            inputRef.current.focus();
        }
        return (
            <div>
                <input type="text" ref={inputRef} />
                <button onClick={onClick}>聚焦</button>
            </div>
        )
    })

    const UseRef = () => {
        const input1 = useRef(null);
        const input2 =createRef()
        let input3 = null;

        const _input1Click = () => {
            input1.current.focus()
        }

        const _input2Click = () => {
            input2.current.focus()
        }

        const handerInput = (input) => {
            input3 = input
        }
        const _input3Click = () => {
            input3.focus()
        }

        // 父组件让子组件组件中的input聚焦
        const inputRef = createRef();
        const onClick = () => {
            inputRef.current.focus();
        }

        return(<>
            <div>
                input1: <input type="text" ref={input1} / >
                <button onClick={_input1Click}>input1自动获取焦点</button>
            </div>
            <div>
                input2: <input type="text" ref={input2} / >
                <button onClick={_input2Click}>input2自动获取焦点</button>
            </div>
            <div>
                input3:<input type="text" ref={handerInput} />
                <button onClick={_input3Click}>input3自动获取焦点</button>
            </div>
            <br />
            <h7>子组件</h7>
            < ChildComp ref={inputRef} />
            <h7>父组件</h7>
            <button onClick={onClick}>父组件让子组件的input聚焦</button>
        </>)
    }
```

### useContext
- `context` 是在外部 `create` ，内部 `use` 的 `state`，它和全局变量的区别在于，如果多个组件同时 `useContext`，那么这些组件都会 `rerender`，如果多个组件同时 `useState` 同一个全局变量，则只有触发 `setState` 的当前组件 `rerender`。
- 如果需要在组件之间共享状态，可以使用`useContext()`
- 可以用来完成类似 `vuex`的全局数据管理

```js
    import React, { useState, useContext, createContext } from 'react';

    const CustomContext = createContext({})

    const UseContext = () => {
        const [username, setUsername] = useState('李四')
        return(<>
            <input value={username} onChange={e => setUsername(e.target.value)}/>
            <CustomContext.Provider value={{username}}>
                <Navbar />
                <Message />
            </CustomContext.Provider>
        </>)
    }

    /* 子组件 navbar */
    const Navbar = () => {
        const { username } = useContext(CustomContext)
        return(<p>Navbar: {username}</p>)
    }
    /* 子组件 message */
    const Message = () => {
        const { username } = useContext(CustomContext)
        return(<p>1 message for {username}</p>)
    }
```

### useReducer
- `useReducer` 是 `useState` 更为高级的一种解决方法，需要外置 `reducer` (全局)，通过这种方式可以对多个状态同时进行控制。
- 仔细端详起来，其实跟 `redux` 中的数据流的概念非常接近

```js
    import React, { useState, useReducer } from 'react';

    const reducer = function(state, action) {
        switch(action.type) {
            case 'up':
                return { count: state.count + 1 }
            case 'down':
                return { count: state.count - 1 }
        }
    }

    const UseReducer = () => {
        const [state, dispatch] = useReducer(reducer, {count: 0})
        return(<>
            <button onClick={() => dispatch({ type: "up" })}>+</button>
            <h1>{state.count}</h1>
            <button onClick={() => dispatch({ type: "down" })}>-</button>
        </>)
    }
```

### 现在最后
- 至此`react hooks`中常用的`hook API`我的理解就说完了， 更加高深的等日后理解了在来了了
- 我们在来看一个简单的全局数据管理的`demo`, `useContext` + `useReducer` 替代 `redux`

```js
    import React, { useState, useContext, useReducer, createContext } from 'react'
    const ColorContext = createContext({})
    const UPDATE_COLOR = 'UPDATE_COLOR'

    /* App 组件 */
    const App = () => {
        return(
            <Color>
                <Buttons></Buttons>
                <ShowArea></ShowArea>
            </Color>
        )
    }

    /* Color 组件 */
    const Color = props => {
        const { color } = useContext(ColorContext)

        const reducer = function(state, action) {
            switch(action.type) {
                case UPDATE_COLOR: 
                    return action.color;
                default: 
                    return state;
            }
        }

        const [ color, dispatch ] = useReducer(reducer, 'blue')
        return(<>
            <ColorContext.Provider value={{color, dispatch}}>
                {props.children}
            </ColorContext.Provider>
        </>)
    }

    /* ShowArea 组件 */
    const ShowArea = props => {
        const { color } = useContext(ColorContext)
        return(<div style={{ color: color }}>字体颜色为{color}</div>)
    }

    /* Buttons 组件 */
    const Buttons = props => {
        const { dispatch } = useContext(ColorContext)
        return(<React.Fragment>
            <button 
                onClick={() =>  dispatch({type: UPDATE_COLOR, color: 'red'})}
            >红色</button>
            <button
                onClick={() =>  dispatch({type: UPDATE_COLOR, color: 'yellow'})}
            >黄色</button>
        </React.Fragment>)
    }
```

