---
layout: post
title: Promise对象
tags: Promise
categories: javaScript
---


# Promise


<a name="wXbmf"></a>
# 第一章 Promise
<a name="KF0ek"></a>
## 1.1概念
Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理且更强大。从语法上说，Promise 是一个对象，通过它可以获取异步操作的消息。Promise顾名思义为承诺、许诺的意思，意思是使用了Promise之后他肯定会给我们答复，无论成功或者失败都会给我们一个答复。
<a name="0jPIY"></a>
## 1.2用法
<a name="p5b6h"></a>
### 1.2.1 Promise状态
Promise含有三个状态。<br />l Pending（进行中）<br />l Resolved（成功）<br />l Rejected（失败）<br />只存在两种状态变化，进行中 > 成功，进行中 > 失败。
<a name="vnr7r"></a>
### 1.2.2 resolve与reject
Promise构造函数接受一个函数作为参数，该函数含有两个参数分别时resolve、reject，它们是两个函数。<br />l resolve的作用是将Promise对象状态由”进行中” 变为 “成功”。在异步操作完成后调用。<br />l reject 的作用是将Promise对象状态由”进行中” 变为 “失败”。在异步操作失败后调用。
<a name="5sjc6"></a>
### 1.2.3 then
promise对象还有一个比较常用的then方法，用来执行回调函数，then方法接受两个参数，第一个是成功的resolved的回调，另一个是失败rejected的回调，第二个失败的回调参数可选。并且then方法里也可以返回promise对象，这样就可以链式调用了。
<a name="57FdC"></a>
## 1.3 简单使用

```javascript
function fun2(){
		let time=new Date().getTime();
		console.log("fun1函数执行");
		return new Promise(function(resolve,reject){
			setTimeout(function(){
			console.log("延时执行函数得到执行。。。");
			resolve(time);
		},1000);
		});
	}
	
	fun2().then(function(time){
		console.log("time "+time);
	});
//打印结果
 fun1函数执行
 延时执行函数得到执行。。。
 time 1579442314592
```
	<br />代码分析：函数fun2返回了一个Promise对象，构造Promise对象时传入了一个函数，函数使用setTimeout模仿了一个异步操作。延时1秒后执行，并调用resolve并传入参数time改变Promise的状态。
<a name="rgkOi"></a>
## 1.4 更多使用
<a name="ax9j1"></a>
### 1.4.1 then与catch
当异步程序在执行过程中出错时我们可以使用reject回调函数执行错误逻辑。上边的例子我们已经演示了resolve的使用(then函数的第一个参数)。下面我们来演示一下使用reject的两种方式。<br />（1）使用then的第二个参数

```javascript
function fun2(){
		let time=new Date().getTime();
		console.log("fun1函数执行");
		return new Promise(function(resolve,reject){
			setTimeout(function(){
			console.log("延时执行函数得到执行。。。");
			reject(time);
		},1000);
		});
	}
	
	fun2().then(function(time){//resolve
		console.log("resolve time "+time);
	},function(time){//reject
		console.log("reject time "+time);
	});
//控制台打印
 fun1函数执行
 延时执行函数得到执行。。。
 reject time 1579443673344
```
	<br />（2）使用catch

```javascript
function fun2(){
		let time=new Date().getTime();
		console.log("fun1函数执行");
		return new Promise(function(resolve,reject){
			setTimeout(function(){
			console.log("延时执行函数得到执行。。。");
			reject(time);
		},1000);
		});
	}
	
	fun2().then(function(time){//resolve
		console.log("resolve time "+time);
	}).catch(function(time){//reject
		console.log("reject time "+time);
	});
//控制台打印
 fun1函数执行
 延时执行函数得到执行。。。
 reject time 1579445706131
```

<a name="E5k6m"></a>
### 1.4.1 all与race
Promise.all([])方法传入一个Promise数组，数组内所以的Promise对象执行成功后再统一调用resolve方法。

```javascript
function fun1(){
		return new Promise(function(resolve,reject){
			setTimeout(function(){
				console.log("fun1");
				resolve("fun1");
		},1000);
		});
	}
			
	function fun2(){
		return new Promise(function(resolve,reject){
			setTimeout(function(){
				console.log("fun2");
				resolve("fun2");
		},1500);
		});
	}
	
	function fun3(){
		return new Promise(function(resolve,reject){
			setTimeout(function(){
				console.log("fun3");
				resolve("fun3");
		},2000);
		});
	}
	
	Promise.all([fun1(),fun2(),fun3()])
			.then(function(funName){
				console.log(funName);
			});
//控制台打印
 fun1
 fun2
 fun3
 ["fun1", "fun2", "fun3"]
```
	<br />Promise.race([])与all类似也是传入一个Promise数组，数组内任意一个Promise对象执行完成就执行resolve函数，未完成的Promise对象将不会得到执行resolve函数的机会，常用于竞争型任务。

```javascript
Promise.race([fun1(),fun2(),fun3()])
			.then(function(funName){
				console.log(funName+" 竞争成功");
			});
//控制台打印
fun1
fun1 竞争成功
fun2
fun3
```
	<br />
