---
title: ES6语法
date: 2017-11-30 09:36:29
tags: es6
categories: 编程

---
### ES6 全称 ------  ECMAscript 6
- 在浏览器中符合使用：
	- 需要用编译器
		- babel
		- traceur  -----有Google出的编译器，把ES6语法编译成ES5 
		- boostrap ----引导程序  和CSS框架 boostrap不一样
- 用法1：
	- 在线编译----主要用于测试
- 用法2：
	- 直接在node里面使用
		- 直接用，需要添加‘use strict’
		- node --harmony_desctructuing xx.js  

### 新增的功能
- 定义变量
	- var a = 12；
	- let  用来定义变量   let a =12；
	- let和var 定义变量的区别
		- 代码块 {}包起来的代码 形成了一个作用域，块级作用域  比如for、if、while
		- let定义的变量只能在代码块里面使用
		- var 定义的变量只有函数作用域
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-1.png)</br>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-2.png)</br>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-3.png)</br>
</div>
		- let具备块级作用域   块级作用域就是一个匿名函数立即调用 
		- let定义的变量不允许重复声明
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-4.png)</br>
</div>
		- 总结：let才接近其他语言的变量
	- let用处：
		- 封闭空间
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-5.png)</br>
</div>
        - let在同作用域中不可以重复定义;

- 定义常量const  一旦被赋值以后就再也不能被修改了
	- 注意：const必须给初始值，因为以后就没法赋值了，所以声明的时候一定得有值
	- 不能重复的声明
	- 用途为了防止意外修改变量；比如引入的库名，组件名
	
- 字符串链接
	- 之前：采用"+str+"实现  var str = '' 或者 var str = ""
	- 现在：反单引号  数字1 前边的按键
		- var a = ``  字符串模板
		- ${a}就可以
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-6.png)</br>
</div>

- 解构赋值
	- var [a,b,c] = [12,5,14]
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-7.png)</br>
</div>
	- 与JSON配合
		- var {a,b,c} = {a;12,b;14,c;15}
		- var {a,b,c} = {b;14,a;12,c;15}  跟顺序无关
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-8.png)</br>
</div>
	- 模式匹配:
		- var [a,[b,c],d] = [12,[1,2],5]
		- console.log(a,b,c,d)//12,1,2,5

		- var [{a,e},[b,c],d] = [{e:'eee',a:'aaaa'},[1,2],5]
		- console.log(a,b,c,d)//aaa,1,2,5,eee
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-9.png)</br>
</div>
		- json对象的属性名不要修改
	- 解构赋值还可以给个默认值：
```javascript
    var {time=12} = {}  console.log(time)  //12
    // 用法：
    function（obj，json，options）{
        options = options || {}
        options.time = option.time || 300
        }
    function（obj，json，{time = 300} = {}）{
    
    }
```

- 复制数组
```javascript
    var arr = [1,2,3]
    arr1 = arr 
    arr1.pop() 
    console.log(arr,arr1)  // [1,2],[1,2] 引用    方法不可取
    // a、循环
    for(var i = 0; i < arr.length;i++){
    arr1.push(arr[i])
    }
    arr.pop()
    console.log(arr,arr1) //[1,2],[1,2,3]
    // b、Array.from(arr)实现数组复制
    var arr1 = Array.from(arr)
    arr.pop()
    console.log(arr,arr1)  //[1,2],[1,2,3]
    // c、var arr1 = [...arr]
    arr.pop()
    console.log(arr,arr1)  //[1,2],[1,2,3]
```
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-10.png)</br>
</div>

- 循环
	- for of 循环  遍历（迭代）整个对象  表现类似于 for in  循环数组和Map对象
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-11.png)</br>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-12.png)</br>
</div>
	- for of 循环可以循环数组，不可以循环json 
		- 真正的目的为了循环Map对象
	- Map对象:
		- 和json相似，也是一种key-value形式
		- Map对象为了和for of循环配合
```javascript
	var map = new Map()
	//设置
	map.set(name,value')
	//获取
	map.get(name)
	//删除
	map.delete(name)  //返回值是一个布尔值
```
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-13.png)
</div>
	    - 遍历Map不能使用 for in，没有效果  使用 for of
```javascript
    for（var name of map）{
        console.log（name）
    }
    for（var [key,value] of map）{
        console.log（key,value）
    }
```
		- 默认循环的是
```javascript
    for（var name of map.entries（））{
        console.log（name）
    }
    for（var [key,value] of map.entries（））{
        console.log（key,value）
    }
```
		- 循环只输出单个  key  或 value
```javascript
    for（var key of map.keys（））{
        console.log（key）//a,b,c,d
    }
    for（var val of map.values（））{
        console.log（val）
    }
```
	- for of 循环 数组  默认循环的是值，要想循环缩影 for（var key of arr.keys())
	- 索引和值都循环：for (var some of arr.entries()){}

- 箭头函数
	- 注意：
		- this问题  this指向的是window 
		-  arguments是不可以使用的
```javascript
    var json = {
        a:2,
        b:3,
        show:()=>{
            console.log(this.a)//报错  undefined
        }
    }
    json.show();
    
    function show(){
        console.log(arguments)
    }
    show()//[1,2,3]
    
    var  show = ()=>{
        console.log(arguments)
    }
    show(1,2,3)//arguments is not defined
```

- 对象
	- 对象语法简洁化
	- 单体模式
```javascript
    var name = 'ls';
    var age = 101;
    var person = {
        name,
        age,
        showName(){
            return this.name
        },
        showAge(){
            return this.age
        }
    }
    console.log(person.showName())
```
	- 面向对象
```javascript
    // es6之前
    function Person(name,age){  //即是类  又是构造函数
        this.name = name;
        this.age = age
    }
    Person.prototype.showName = function(){
        return this.name
    }
    Person.prototype.showAge = function(){
        return this.age
    }
    var p1 = new Person('abc',101)
    console.log(p1.showName())
```
	
```javascript
// ES6中彻底区分开了
// 	  类class
// 	  构造函数 constructor  生成完实例以后，就自己执行的函数

class Person{  //这才是一个真正的类
    constructor(name,age){
        this.name = name;
        this.age = age
    }
    showName(){
        return this.name
    }
    showAge(){
        return this.age
    }
}
var p1 = new Person('aaa',101)
var p2 = new Person('bbb',102)
console.log(p1.name)
console.log(p1.showName())

console.log(p1.showName == p2.showName) //true
p1.constructor == Person  //true
```
	- 函数给默认值
```javascript
    function move(obj='对象是必须要填的',json,options){
        console.log(obj)
    }
    move()// 对象是必须要填的
```

- 继承
	- 原型继承
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-14.png)
</div>

```javascript
    function Worker(name,age){
        Person.apply(this,arguments)
    }
    Worker.prototype = new Person()
    var w1 = new Worker('aaa',101)
    console.log(w1.showName())//aaa
```
    - es6之前  子类.prototype = new 父类();
    - ES6  继承  class Worker extends Person{}
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-15.png)
</div>

```javascript
    class Worker extends Person{
        constructor(){
            super()//再次调用父级构造
        }
    }
```
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-16.png)
</div>
	- 应用：队列类
<div align=center>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-17.png)</br>
![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/es6/es6-18.png)
</div>

- 模块化：
	- seajs    requirejs
	- ES6 自带模块化   `----必须引入 traceur.js 和 boostrap.js  <script type = 'module'></script>`
```javascript
	//如何定义（导出）模块：
	const a =12;
	export default a ;
	
	//导出多个模块
	const a = 12;
	const b = 13;
	export default {a,b}  
	//如何使用（引用）：
	import modA from './a.js'
```

- Promise    承诺的意思
	- 就是一个对象，用来去传递异步操作的数据（消息）
	- 异步：多个操作可以同时进行  Ajax  
		- pending（等待，处理中）---> Resolve（完成、fullFiled）--->Rejected（拒绝，失败）
		
    ```javascript
    var p1 = new Promise(function(resolve,reject){   //  回调函数会立即执行
        //resolve 成功了
        //reject 失败了
    })
    
    var p1 = new Promise(function(resolve,reject){
        if(异步处理成功了){
            resolve(成功的数据)
        }else{
            reject(失败的原因)
        }
    })
    
    p1.then(成功(resolve),失败(reject))  //返回的还是一个Promise对象 可以继续往后走
```
- 案例：

```javascript
    var p1 = new Promise(function(resolve,reject){   //  回调函数会立即执行
        resolve（1）
    })；
    p1.then(function(value){  //value 数据
            alert('成功了'+ value)
        },function(){
            alert("失败了")
    })
    
    
    var p1 = new Promise(function(resolve,reject){   //  回调函数会立即执行
        reject（2）
    })；
    p1.then(function(value){  //value 数据
            alert('成功了'+ value)
        },function(){
            alert("失败了"+ value)
    })
    
    var p1 = new Promise(function(resolve,reject){   //  回调函数会立即执行
        resolve（1）
    })；
    p1.then(function(value){  //value 数据
        return value +1；
        },function(){
            alert("失败了")
        }).then(function(value){
            console.log(value) // 此处的value就是 上一个成功return的数据
    })
```

 - catch    --->用来捕获错误

```javascript
	var p1 = new Promise(function(resolve,reject){
		resolve('成功了')
	})
	p1.then(function(value){
		console.log(value)
		throw '出现错误了'   //抛出错误的
	}).catch(function(e){
		console.log(e)
	})  //  成功了  出现错误了
```

- all ----全部  用于将多个promise对象组合，包装成一个全新的promise实例
	- 用法：Promise.all([p1,p2,p3])
		- 所有的promise对象都正确走成功，否则只要有一个错误，是失败了
		
```javascript
    var p1 = Promise.resolve(3)
    var p2 = Promise.reject(5)
    Promise.all([true,p1,p2]).then(function(value){
        console.log('成功了'+value)
    },function(value){
        console.log('错误了'+value)
    })
```
- race   ————返回也是一个promise对象
	- 最先能执行的promise结果  哪个最快就用哪个

```javascript
    var p1 = new Promise(function(resolve,reject){
        setTimeout(resolve,500,'one')
    })
    var p2 = new Promise(function(resolve,reject){
        setTimeout(resolve,100,'two')
    })
    Promise.race([p1,p2]).then(function(value){
        console.log(value) //two
    })
```
-  reject   resolve
	- Promise.reject()    生成一个错误的promise对象
	- Promise.resolve()   生成一个成功的promise对象

```javascript
    Promise.reject('这是错误的信息').then(function(){
    
    },function(res){
        console.log(res)  //这是错误的信息
    })
```

- Promise.resolve()
	- 语法： Promise.resolve(value)
	- Promise.resolve(promise)

```javascript
    var p1 = Promise.resolve(3)
    var p2 = Promise.resolve(p1)
    p2.then(function(value){
        console.log(value)	//3
    })  
```

