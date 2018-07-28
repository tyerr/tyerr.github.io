---
layout: post
title:  "JS模块化"
categories: AMD CMD CommonJS
tags:  AMD CMD CommonJS
author: TY
---

* content
{:toc}
详细介绍了JS模块化




/*

# JS模块化
# 一、模块化
## 1.说说你对AMD和Commonjs的理解  
#### CommonJS是服务器端模块的规范，Node.js采用了这个规范。CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数  
## 2.Commonjs和ES6模块化的区别  
#### CommonJS是服务器端模块的规范，Node.js采用了这个规范。但目前也可用于浏览器端，需要使用Browserify进行提前编译打包.（CommonJS是服务器端模块的规范，Node.js采用了这个规范。但目前也可用于浏览器端，需要使用Browserify进行提前编译打包）  
#### ES6模块化规范专门针对于浏览器端，但目前浏览器支持不是很好(只有Chrome浏览器支持), 也需要使用Browserify进行打包，或者使用Babel将ES6编译成ES5代码
#### 暴露的方式和暴露的本质
	1、commonjs暴露的方式
	  - module.exports = value;
	  - exports.xxx = value;
	  - 暴露的本质是exports对象
	2、ES6中暴露的方式
	  - export xxx （常规暴露，暴露的本质是对象，接收的时候只能以对象的解构赋值的方式来接收值）
	  - export default （默认暴露，暴露任意数据类型，暴露什么数据类型，接收什么数据类型）
## 3.谈谈你对js模块化的理解
#### 1)当工业级的项目开发的足够大的时候，如果将所有的js代码定义在一个js文件的话，使得复杂度提升，后期维护难度加大。  
#### 2)如果将一个大的js文件根据一定的规范拆分成几个小的文件的话将会便于管理，可以提高复用性。
#### 3)模块化在项目中十分的重要，一个复杂的项目肯定有很多相似的功能模块，如果每次都需要重新编写模块肯定既费时又耗力。但是引用别人编写模块的前提是要有统一的“打开姿势”，如果每个人有各自的写法，那么肯定会乱套，所有会引出模块化规范的使用
#### 4)常用的JavaScript模块化规范有四种： Commonjs， AMD(require.js), CMD(sea.js), ES6模块化
# CommonJS  
![commonjs](https://i.imgur.com/9WgcAzM.png)  
# CMD（SeaJS）  
![CMD](https://i.imgur.com/RfJyy6Z.png)  
# AMD（RequireJS）  
![AMD](https://i.imgur.com/5Jtax8B.png)  
#ES6  
![ES6](https://i.imgur.com/r8jsjJb.png)  
# 二、模块化的理解
## 1.什么是模块？
#### 将一个复杂的程序依据一定的规则(规范)封装成几个块(文件), 并进行组合在一起块的内部数据/实现是私有的, 只是向外部暴露一些接口(方法)与外部其它模块通信  
## 2.一个模块的组成  
#### 数据（内部的属性）+ 操作数据的行为（内部的函数）
## 3.模块化  
#### 编码时按照模块一个一个编码，整个项目就是一个模块化的项目  
## 4.模块化的好处  
- 避免命名冲突（减少命名空间污染）
- 更好的分离，按需加载
- 更高的复用性
- 高可维护性  
## 5.没有模块化，页面加载script的问题：
- 请求过多
- 依赖模糊
- 难以维护
## 6.模块化的进程
#### 全局function模式
- 编码：全局变量/函数
- 问题: 污染全局命名空间, 容易引起命名冲突/数据不安全
#### namespace模式
- 编码: 将数据/行为封装到对象中  
- 解决: 命名冲突(减少了全局变量)  
- 问题: 数据不安全(外部可以直接修改模块内部的数据)  
#### IIFE模式/增强  
- IIFE : 立即调用函数表达式--->匿名函数自调用  
- 编码: 将数据和行为封装到一个函数内部, 通过给window添加属性来向外暴露接口  
- 引入依赖: 通过函数形参来引入依赖模块   

```
(function(window, module2){  
 var data = 'atguigu.com'   
	function foo() {   
			module2.xxx()   
			console.log('foo()'+data) }   
	function bar() {   
	console.log('bar()'+data) }
	window.module = {foo} })(window, module2) 
```