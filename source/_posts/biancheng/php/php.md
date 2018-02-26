---
title: php
date: 2017-11-30 09:11:35
tags: php
categories: 编程

---
### 变量、数组声明
- $num $Num 区分大小写
- $arr = array(1,2,3,4,5)

### 输出语句
- echo 简单数据
- print_r 复杂数据 数组也可以  echo $arrp[0]去访问
- var_dump()  输出数据 类型

### 字符串拼接
- 用 . 进行拼接 不是JS中的+ 

### 单双引号有区别
- 单引号不识别变量，遇到变量依旧按照字符串输出，双引号识别变量

### 数组遍历
```javascript
	$arr  = array(1,2,3,4)
	方法一：
	for($i = 0;$i<count($arr);$i++){
		echo $arr[$i]
	 }
	方法二：
	foreach($arr as $value){
		echo $value
	 }
	方法三：
	foreach($arr as $key =>$value){
		echo $key.'====='.$value   //索引 ==== 值
	 }
```

### 函数：与JS中的写法类似  变量要加$num
- PHP 中也是有预解析的
```javascript
	//自定义函数     函数名不区分大小写
	function foo（$info）{
		return $info
	}
	$num = foo()
	echo $num
	//系统函数
	json_encode()   //将JSON格式的字符串转换成对象  
```

### 预定义变量（表达处理）前后端交互
```javascript
$_GET[]  获取到的时URL中传递的参数
url：http://localhost/php/test/01.php?abc=123
	<?php 
		$f = $_GET['abc'];
		echo "<div>".$f."</div>"; //123
	?>
```

### http协议的常用请求方式（增删改查）
- get 用来从服务器获取参数（参数一般作为查询条件）
- post 用来添加数据
- put 用来修改数据
- delete 用来删除数据

### form 表单默认是get请求  想要是想post请求 需要在method= ’post‘
<div align=center>
<img src='http://p040q6o73.bkt.clouddn.com/image/boke/php/php-1.png'>
<div/>
<div align=center>
<img src='http://p040q6o73.bkt.clouddn.com/image/boke/php/php-2.png'>
<div>