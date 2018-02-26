---
title: Angular
date: 2017-11-29 17:00:44
tags: angular
categories: 编程

---
# Angular

## Angular 与 jQuery
- jQuery: 库
    + 提供了一些已有的方法，我们主动调用它。
    + $('.test').val()

- Angular: 框架
    + 框架提供了一种结构或者模式
    + 我们按照它提供这种结构或者模式去书写代码
    + 框架会帮助我们去剩下的事情

### Angular简介
- 一款非常优秀的前端高级 JS 框架
- 最早由 Misko Hevery 等人创建
- 2009 年被 Google 公式收购，用于其多款产品
- 目前有一个全职的开发团队继续开发和维护这个库
- 有了这一类框架就可以轻松构建 SPA 应用程序
- 其核心就是通过指令扩展了 HTML，通过表达式绑定数据到 HTML。

#### 什么是 SPA
- single page application
- 单页应用程序

### 值+1

### 指令
- 在angular中以ng-开头的html标签属性，称之为指令
- ng-app: 选择angular去管理哪一部分的html代码, 管理的是ng-app所在
   元素的子元素及其本身
- ng-click: 也是用来注册点击事件
- ng-model: 可以指定一个属性值，这个属性就表示当前文本框的value值
- ng-init: 可以对数据进行初始化操作，给一个默认值。

### 模块
- 创建模块:`var app = angular.module('模块名',[])`
- *注意* 如果不依赖其他模块，也需要提供一个空的数组
- 需要在ng-app指令的属性值上写我们的模块名(房子)

### 控制器
- 创建控制器:`app.controller('控制器的名字',function($scope){})`
- 如果要改变页面{{testName}}的值，就直接改变$scope.testName值就可以
- *注意* 要显示数据模型的值,就要在它所在标签或者父标签上加上ng-controller指令
  ng-controller的值就是我们想要显示的数据模型所在控制器的名字
  :ng-controller="window"

### 双向数据绑定
- 数据模型($scope.属性)的改变，会影响内容的显示(文本框的值)
- 我们改变了文本框的值，对应的数据模型发生了改变.
- 这种相互影响的关系就称之为双向数据绑定.

### MVC 思想
- M : Model: 存储，获取数据
- V : View 视图，把数据呈现给用户
- C : Controller 做一些控制和调度的操作

## 自定义指令
- 自定义指令无外乎增强了HTML,提供了额外的功能。
- 内部指令基本能满足我们的需求。
- 少数情况下我们有一些特殊的需要，可以通过自定义指令的方式实现

## 创建自定义指令
- *注意: 名字要符合驼峰合法*

```javascript
    // 1.创建模块对象
    var app = angular.module('directiveApp', [])

    // 2.创建自定义指令
    // 第一个参数：是指令的名字,必须是驼峰命名法
    //             使用时把大写改成小写，在原来大写前加上-
    // 第二个参数：和控制器的第二个参数类似!
    app.directive('myBtn', [function(){
      // 在这里直接return 一个对象就可以了
      return {
        // template属性，是封装的ui
        template:'<div><button>我是按钮</button></div>'
      }
    }])
```

### 自定义指令中回调里返回的对象的属性
- template: 需要提供一个html字符串,最终会被添加到当前页面使用了自定义指令的位置
- templateUrl:需要提供一个html文件路径，angular会发请求，请求对应的文件，
 把文件内容作为模板插入到自定义指令中间,也可以提供一个script标签的id, angular会把script标签中的内容作为模板插入到自定义指令中间,*注意* 要改变script标签的type="text/ng-template"

- restrict: 也是需要提供一个字符串，限制自定义指令的使用形式
    + A : Attribute 表示只能以属性的形式使用
    + C : Class     表示只能以类样式名的形式使用
    + E : Element   表示只能以自定义标签的形式使用
    + ACE : 如果希望多种方
    + 式合适，就把对应值组合在一起。

- replace：需要提供一个布尔值，为true时，模板会被用来替换自定义指令所在标签，
    * 否则是插入到自定义指令中间
        
- transclude: 需要一个布尔值, 为true时会将自定义标签中的内容插入到模板中，
    * 拥有ng-transclude 指令的标签中间

- scope：需要一个对象: 可以用来获取自定义指令的属性值,
    -  给当前对象添加一个属性(如:tmp),属性值以@开头，后面跟上自定义指令的属性名
       然后就可以在模板中使用{{tmp}} 来得到对应的属性值
       + `scope: { tmp:'@mystyle'}`  {{tmp}}
       + `scope: { mystyle:'@'}`     {{mystyle}}

- link: 需要一个function 这是function在angular解析到相应指令时就会执行一次,
    + scope      ：类似于$scope,只不过scope的属性只能在模板中使用
    + element    : 自定义指令所在标签对应的对象(jqLite)
    + attributes : 包含了自定义指令所在标签的必有属性

## 过滤器(filter)
- 格式化数据
- 过滤数据(filter)

```html
    <ul>
        <!--  如果指定一个布尔值，或者字符串就是全文匹配 -->
      <!-- 会到对应的todos中寻找，如果当前元素有completed属性且值 为true就会被显示出来。（只会到completed属性中寻找） -->
      <li ng-repeat="item in todos | filter : {completed:true} ">
        {{item.name}},{{item.completed}}
      </li>
    </ul>
```


```html
  <h1>currency</h1>
  <!-- 在数据模型后加上|  再加上过滤器的名字 
        也可以在过滤器名字后指定参数，参数是写在冒号后面的-->
  <p>{{money | currency : '￥'}}</p>

  <h1>date</h1>
  <p>{{myDate | date : 'yyyy年MM月dd日 HH:mm:ss'}}</p>
```

- limitTo

```html
    <h1>limitTo</h1>
  <!-- 第一个参数，表明显示多少个字，第二个参数表示，从第几个字开始显示(索引从0开始) -->
  <p>{{msg | limitTo : 5 : 2}}...</p>
```

- orderBy 及 json

```html
<h1>json</h1>
 <!--  格式化显示json数据，参数指定缩近的长度 -->
 <pre>{{myJson | json : 8}}</pre>
  <h1>orderBy</h1>
  <!-- 对数据进行排序，参数，给+号就按正序排，- 就按倒序排 -->
  <span ng-repeat="item in arr | orderBy:'-'">{{item }}，</span>
```

- 在js中使用过滤器

```javascript
    <!-- $filter 需要在控制器的回调中传入 -->
    // 可以调用不同的过滤器得到相应的结果
      // 参数是一个过滤器的名字
      // 返回值是一个方法
      //        : 第一个参数是需要处理的数据
      //        : 后面的参数是当前过滤器本身需要的参数
     $scope.result =  $filter('currency')($scope.money,'￥')

```

## todomvc使用filter过滤器来切换不同状态任务的显示
- arr=[{a:3,b:4},{a:5}]
- 数据模型 | filter : {a:3}

## $watch 监视数据模型的变化

```javascript
    $scope.name = '小明'
      $scope.age = 18

      // $watch可以用来监视数据模型的变化
      // 第一个参数: 数据模型对应的名字(字符串形式)
      // 第二个参数: 相应的数据模型变化就会调用 这个函数
      // 默认会直接执行一次回调函数
      $scope.$watch('name',function(now,old){
        // 第一个参数是变化后的值
        // 第二个参数是变化前的值
        // console.log(now,old)
      })
```
- 也可以监视方法的返回值

```javascript
    $scope.getAge = function(){
        return $scope.age
      }
      
      // 也能够监视$scope.属性中的方法的返回值
      $scope.$watch('getAge()',function(now,old){
        console.log(now,old)
      })
```

## 服务
- 创建服务

```javascript
    // 1.创建服务模块
  var app = angular.module('service',[])

  // 2.创建服务
  // 第一个参数：服务的名字
  // 第二个参数里的function: 
  //    angular会把这个function当作构建函数，angular会帮助我们new这个构建函数，然后得到一个对象。A,B
  app.service('MyService', [function(){
    this.name = '小明'
  }])
```

- 使用服务

```javascript
    // 1.创建模块
  var app = angular.module('todosApp', ['service'])
  // 2.创建控制器
  app.controller('todosController', [
    'MyService'
    , function(MyService){
    // 这个MyService就是，对应的'MyService'时的回调函数new出的对象
    console.log(MyService)
}])
```

## 路由
- 根据url中不同的锚点值，在页面显示不同的内容！

### 路由使用

```javascript
    // 1.创建模块
    var app = angular.module('myApp', ['ngRoute'])

    // 2.配置路由规则(约定什么样的锚点值，对应什么样的内容)
    // 第一个参数与controller第二个参数类似
    app.config(['$routeProvider',function($routeProvider){
      
      // 第一个参数：对应的url中锚点值
      // 当angular检测到url中锚点变为/ali里，就会把template对应的内容插入到页面拥有ng-view指令的标签中
      $routeProvider.when('/ali',{
        template:'<div>阿里在二楼!</div>',
        // 指定一个控制器的名字,
        // 当前url中锚点值为/ali时就会执行控制器中的回调函数
        // 我们可以直接在template/templateUrl对应的模板中使用该控制器中对应的$scope属性值
        controller:'demoController'
        //templateUrl
      })
      .when('/baidu',{
        template:'<div>百度在1楼</div>'
      })
    }])
```

### 路由参数
- 在原有规则中可以加冒号:,
- 表示:号后的内容是可以变的(但是必须要有)
    + 如果加上问号，就能够匹配到了
- 在控制器中可以通过$routeParams得到myname对应的值

```javascript
    $routeProvider.when('/company/:myname？')
```

### otherwise
- 当不匹配when方法对应的规则时，就会匹配otherwise对应的规则


## webapi
- I/O
-  // url

## api
- 
- console.log('小明') //小明
- I/O     docuemnt.getGetElementById('')

## 豆瓣API

- ng-src 指令: 用来取代src

### $http
- 发请求

```javascript
    http.get('./in_theaters/data.json')
      .then(function(res){
        // 成功的回调函数
        // console.log(res.data)
        $scope.data = res.data
      })
```


## 跨域
- img  src可以指向一个跨域的资源
- link
- video audio .

- script: src 一般指向一个js文件
- a.com 请求b.com/tmp.js文件

-A 版本：
- 假设tmp.js 内容为: var data = 123
<script src="b.com/tmp.js"></script>
<script>
    console.log(data)
</script>

-B 版本
- 假设tmp.js 内容为: var data = {name:'小明',age:18}
<script src="b.com/tmp.js"></script>
<script>
    console.log(data)
</script>

-C 版本
- 假设tmp.js 内容为:  test({name:'小明',age:18})
<script>
    function test(data){
        console.log(data)
    }
</script>
<script src="b.com/tmp.js"></script>