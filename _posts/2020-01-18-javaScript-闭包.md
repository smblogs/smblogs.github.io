---
layout: post
title: javaScript闭包
tags: javaScript
categories: javaScript
---


# 闭包

<a name="Jpa2O"></a>
# [第]()一章 JavaScript闭包
<a name="Cy1mi"></a>
## [1.]()1概述
在JS高阶函数中，函数可以接受一个函数作为参数，同时可以返回一个函数。当函数的返回是一个函数且返回的函数保存函数变量的某个引用，那么这个变量将不会被回收，外界便可以通过返回的函数操作函数内部的变量。一个很抽象的概念我们来用代码演示一下。
<a name="byJJv"></a>
## 1.2闭包演示

```javascript
	function fun1(val) {
		let num = val;
		return function fun2() {
			num++;
			return num;
		}
	}
	var fun = fun1(5); //传入5 num=5
	console.log(fun()); //6
	console.log(fun()); //7
```

如上代码: 我们执行fun1()函数后返回了函数fun()即fun1。当我们多次执行fun()方法得到的结果确并不一致。这就是JS的闭包导致。返回的函数任然保存着对num变量的引用。我们多次执行时num会不断累加。
<a name="V5NAU"></a>
## 1.3闭包与循环
<a name="g56Sv"></a>
### 1.3.1代码演示
为了加深我们的对闭包的理解，我们在来演示一个闭包的例子

```javascript
function count() {
var arr = [];
for(var i = 1; i <= 3; i++) {
arr.push(function() {
return i * i;
});
}
return arr;
}
let results=count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];
console.log(f1());
console.log(f2());
console.log(f3());
```
<br />看完代码后让我们对打印的结果预测一下，1 4 9。<br />然而实际的结果打印结果: 16 16 16。<br />为什么会出现这种结果呢，原因是这样的，闭包问题内返回的函数是不会立即执行的，它就是一个变量，当它被调用执行时才会被执行，上边的例子中在执行f1(),f2().f3()时变量i此时已经是4了，所以打印结果为16 16 16。<br />那么我们如何解决闭包与循环结合产生的问题呢：
<a name="1lScW"></a>
### 1.3.2解决方案1
****创建一个匿名函数并立刻执行****

```javascript
function count() {
var arr = [];
for(let i = 1; i <= 3; i++) {
arr.push((function(n){
return function(){
return n*n;
}
})(i));
}
return arr;
}
let results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];
console.log(f1()); //1
console.log(f2()); //4
console.log(f3()); //9
```

<a name="MsvwG"></a>
### 1.3.3解决方案2
****使用Let，****ES6语法中Let在循环内部有特殊的定义****，我们可以理解为：在for(let i = 1; i <= 3; i++) 中，即圆括号内建立了一个隐藏的作用域，每一次循环都像创建了一个新变量。<br />详见：[https://segmentfault.com/a/1190000014951691?utm_source=tag-newest](https://segmentfault.com/a/1190000014951691?utm_source=tag-newest)

```javascript
function count() {
var arr = [];
for(let i = 1; i <= 3; i++) {
arr.push(function() {
return i * i;
});
}
return arr;
}
let results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];
console.log(f1()); //1
console.log(f2()); //4
console.log(f3()); //9
```



 <br />
<br /> <br /> <br /> <br /> <br /> <br /> <br /> 

  



