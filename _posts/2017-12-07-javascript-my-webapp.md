# js基础知识  
## 一、数据类型
### 1.分类（2大类）  
**五种基本（值）类型：简单的数据段**  
* Number: 任意数值  
* String：任意字符串  
* Boolean：true/false
* undefined：undefined
* null：null
**对象（引用）类型：有多个值构成的对象**  
* Object: 任意对象  
* Array: 特别的对象类型(下标/内部数据有序)
* Function: 特别的对象类型(可执行)  
### 2. 判断：typeof，instanceof 与 ===
* typeof：返回的是数据类型的字符串表达形式
	- 可以区别: 数值, 字符串, 布尔值, undefined, function,object
	- 不能区别: null与对象, 一般对象与数组
* instanceof：专门用来判断对象数据的类型: Object, Array与Function
* ===：可以判断: undefined和null
* 特殊：typeof null  为 object  

### 代码分析

```
var obj = { 
  n: [1, true, console, console.log]
}  
console.log(typeof (typeof obj)); //'object'  
console.log(typeof obj.n); //object  
console.log(typeof obj.n[2]) //object  
console.log(typeof obj.n[3]); //function  
console.log(obj instanceof Object);  //true  
console.log(obj.n instanceof Array);  //true  
console.log(obj.n[3] instanceof Function); //true  
console.log(undefined == null); //true（会先做隐式转换）  
console.log(undefined === null); //false（绝对相等）  
```
### 3.相关问题  
* 1）undefined与null的区别?
	- undefined代表没有赋值  
	- null代表赋值了, 只是值为null  
* 2）什么时候将变量赋值为null?
	- 当我想要创建对象，并不知道赋值为什么，这时候定义为null表示我将来作为对象来使用  
	`var a = null`
	- 让a指向的对象成为垃圾对象  
	`var a = null`
### 4.严格区别变量类型与数据类型?
* js的变量本身是没有类型的, 变量的类型实际上是变量内存中数据（变量的值）的类型
* 变量类型:
	- 基本类型: 保存基本类型数据的变量
	- 引用类型: 保存对象地址值的变量
* 如图：
	- var a=1时，js引擎自动分配内存，由于a的变量类型为基本类型，所以将变量名a以及变量a的值保存在栈中
	- var b={},js引擎自动分配内存，由于b的变量类型为引用类型，所以在堆内存中开辟一块内存用来存放这个对象的数据，并且生成一个地址值（通常为0x开头的），同时在栈中开辟一块内存存储b的变量名以及b对应的地址值，借助地址值引用堆中的数据。
	![](https://i.imgur.com/xWRYEAm.png)

### 5.三种出现undefined
* 第一种：定义变量没有赋值
	`var a ;
	console.log(a);`
* 第二种：定义函数没有指定return返回值
	`function fn() {}
	console.log(fn());`
* 第三种：读取对象中未定义属性
	`var obj = {};
	console.log(obj.a);`

## 二、数据, 变量与内存
### 1. 什么是数据以及数据的特点?
* 存储于内存中代表特定信息的'东东', 本质就是0101二进制
* 特点：可读可传递
* 万物(一切)皆数据, 函数也是数据
* 函数具备可读，可传递  例如：
`function（function）{ }`  `//函数可作为参数进行传递，例如jquery原生代码`
* 程序中所有操作的目标: 数据
* 例如：
	- a++，操作的不是a这个变量，而是a的变量值
	- 算术运算
	- 逻辑运算
	- 赋值
	- 调用函数传参
### 2. 什么是内存?
* 内存：内存条通电后产生的存储空间(临时的)
* 产生和死亡: 内存条(集成电路板)==>通电==>产生一定容量的存储空间==>存储各种数据==>断电==>内存全部消失
* 内存的空间是临时的, 而硬盘的空间是持久的
* 内存空间:
![变量存储图](https://i.imgur.com/0Je9lL0.png)
### 3. 什么是变量?
* 变量：值可以变化的量, 由变量名与变量值组成
* 一个变量对应一块小内存, 变量名用来查找到内存, 变量值就是内存中保存的内容
### 4. 内存,数据, 变量三者之间的关系
* 内存是一个容器, 用来存储程序运行需要操作的数据
* 变量是内存的标识, 我们通过变量找到对应的内存, 进而操作(读/写)内存中的数据
### 5.相关问题
* 1）关于赋值与内存的问题?
	- 赋值操作操作的是值，而不是变量，赋值时如果为基本数据类型，存储在栈中，如果为引用类型，存储在堆中，并且将地址值存储在栈中，引用存在堆中的数据
* 2）关于引用变量赋值问题?
`// 2个引用变量指向同一个对象, 通过一个引用变量修改对象内部数据, 另一个引用变量也看得见`

```
var a = {
  n: 123
}
var b = a;
a.n = 456;
console.log(b.n); //456
// 2个引用变量指向同一个对象,让一个引用变量指向另一个对象, 另一个引用变量还是指向原来的对象
var a = {
  n: 123
}
var b = a;
a = {
  n: 456
}
console.log(b.n);  //123
```
* 3）关于数据传递问题?
	- 问题: 在js调用函数时传递变量参数时, 是值传递还是引用传递
	- 答案：只有值传递, 没有引用传递, 传递的都是变量的值, 只是这个值可能是基本数据, 也可能是地址(引用)数据

```
<script type="text/javascript">
  function test(x) {console.log(x);}
  var a = 123;
  test(a); //值传递
  a = {};
  test(a); //值传递，地址值
```

* 4）JS引擎如何管理内存?
	- 1. 内存生命周期
		- 1)分配需要的内存
		- 2)使用分配到的内存
		- 3)不需要时将其释放/归还
	- 2. 释放内存
		- 1）为执行函数分配的栈空间内存: 函数执行完自动释放
		- 2）存储对象的堆空间内存: 当内存没有引用指向时(设置为null), 对象成为垃圾对象, 垃圾回收器后面就会回收释放此内存

## 三、对象
### 1. 什么是对象?
* 代表现实事物在编程中的抽象
* 多个数据的集合体(封装体)
* 用于保存多个数据的容器
### 2. 为什么要用对象?
* 便于对多个数据进行统一管理，减少全局变量的使用，因为全局变量一旦创建，不可删除，影响命名空间
### 3. 对象的组成
* 属性：
	- 代表现实事物的状态数据
* 由属性名和属性值组成
	- 属性名都是字符串类型, 属性值是任意类型

```
例如：
var obj = {
 	 name: '小明'；//其实name为字符串'name'
}
```
* 方法：
	- 代表现实事物的行为数据
	- 是特别的属性==>属性值是函数
### 4. 如何访问对象内部数据?
* 点语法：样式：.属性名		编码简单, 但有时不能用
* 字符串法：样式：['属性名']	 编码麻烦, 但通用
### 5.什么时候必须使用['属性名']的方式?
* 第一种：属性名不是合法的标识名
* 第二种：属性名不确定
* 例如：
	- 情形一: 属性名不是合法的标识名		需求: 添加一个属性: content-type: text/json
	`var p = {}`
 	`p['content-type'] = 'text/json'； //  p.content-type = 'text/json' //不正确`
	- 情形二: 属性名不确定
	
	```
	var prop = 'xxx'
	var value = 123
	p[prop] = value； // p.prop = value  //不正确
	console.log(p['content-type'], p[prop])
	```
* 面试题：
	
	```
	var a = {}
	var obj1 = {n: 2}
	var obj2 = {m: 3}
	a[obj1]=4   //a[obj1.toString()] = 4
	a[obj2]=5   //a[obj2.toString()] = 5
	console.log(a);  // [object Object]:5
	console.log(obj1.toString()); // [object Object]
	console.log(a[obj1]) // 输出多少? 5
	alert({}.toString())  //[object Object]
	```
* 知识点：任何对象调用tostring这个方法都会返回[object Object]，并且[object Object]为字符串类型，并不是数组
* 样式：对象.toString为[object Object]
* 对象【对象】===对象【对象.tostring】
## 四、函数
### 1. 什么是函数?
* 具有特定功能，并且可复用的n条语句的封装体
* 函数也是对象(有这个函数名.prototype，这时称为函数对象)  具有对象的功能与特点：
```
function fn() {}
console.log(fn instanceof Object) // 是Object类型的实例
console.log(fn.prototype) // 内部有属性
console.log(fn.call) // 内部有方法
fn.t1 = 'atguigu' // 可以添加属性
fn.t2 = function () { // 可以添加方法
  console.log('t2() '+this.t1)
}
fn.t2()
```
### 2. 为什么要用函数?
* 提高代码复用
* 便于阅读和交流
### 3. 如何定义函数?
* 1）两种定义方法：
	- 第一种：函数声明，会发生函数提升
`function calc() {}`
	- 第二种：表达式，提升的时calc这个变量名
`var calc = function () { }`
* 2）定义函数需要的注意事项：
	- 1. 传入参数
	- 2. 代码段
	- 3. 返回值
### 4. 如何调用(执行)函数?

```
test()	//直接调用
new test() //new调用
obj.test() //对象点取调用
test.call/apply(obj)//使用call/apply调用函数
```
### 5.回调函数
* 1）什么函数才是回调函数?
	- 回调可用于异步加载的通知机制。
	- 特点：
		- 1. 你定义的函数
		- 2. 你并没有直接调用
		- 3. 最终它执行了
* 例如：有时要在A程序中设置一个计时器，每到一定时间，A程序会得到相应的通知，但通知机制的实现者对A程序一无所知。那么，就需一个具有特定原型的函数指针进行回调，通知A程序事件已经发生。实际上，API使用一个回调函数来通知计时器。
* 2）常见的回调函数?
	- 定时器函数
	- DOM事件函数
	- ajax回调函数(后面学)
	- 生命周期回调函数(后面学)
### 6. IIEF（匿名函数自调用）
* 1）理解
	- 全称: Immediately-Invoked Function Expression 立即调用函数表达式
	- 别名: 匿名函数自调用 / 自调用匿名函数
* 2）作用
	- 隐藏内部实现(模块化)
	- 不污染外部命名空间
* 3）何时使用：需要一个函数只执行一次

```
(function (window) {//这个window为形参
  var a = 123;
  console.log('匿名函数调用了');
})(window)//这个window为实参
//传入window就是为了利用window传送函数内的变量
```
### 7. 函数中的this
* 1）this是什么？ 被调用函数的当前对象
	- 在函数中都可以直接使用this
	- 在定义函数时, this还没有确定, 只有在执行时才动态确定(绑定)的
* 2）如何确定this的值?

```
test()   //window
obj.test()  //obj
new test()  //实例对象
test.call/apply(obj)//obj

console.log(this); //window
var test = function () {

}
/*下面这个函数称为构造函数*/
function Person(color) {
  console.log(this)
  this.color = color;
  this.getColor = function () {
    console.log(this)
    return this.color;
  };
  this.setColor = function (color) {
    console.log(this)
    this.color = color;
  };
}
Person("red"); //this是谁?  window

var p = new Person("yello"); //this是谁?  p

p.getColor(); //this是谁?  p

var obj = {};
p.setColor.call(obj, "black"); //this是谁?  //obj

var test = p.setColor;
test(); //this是谁?  window

function fun1() {
  function fun2() {
    console.log(this);
  }

  fun2(); //this是谁?  //window
}
fun1();

演示构造函数new一个实例对象的过程
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.setName = function (name) {
  this.name = name;
}
var p = new Person('Bob', 18);
console.log(p);


console.log(p.name, p.age);	//Bob 18
p.setName('Jack')
console.log(p);	
```

```
 new关键字做的四件事：
//1. 创建了一个空的对象
  var p = {};
//2. 将实例对象的proto属性等于构造函数的prototype属性
  p.__proto__ = Person.prototype;
//3. 构造函数调用call方法，其中Bob与18就是argument中的内容
  var result = Person.call(p, 'Bob', 18)
//4. 指定返回值，由于Person没有return，所以返回值为underfined
所以返回一个p
  return typeof result === 'object' ? result : p;
```
## 五.分号问题
* 1. js一条语句的后面可以不加分号
* 2. 是否加分号是编码风格问题, 没有应该不应该，只有你自己喜欢不喜欢
* 3. 在下面2种情况下不加分号会有问题
小括号开头的前一条语句
// 情形一: 小括号开头的前一条语句
var a = 3
;(function () {

})()
/*
错误理解: 将3看成是函数调用
 var a = 3(function () {

 })
 */
 中方括号开头的前一条语句
// 情形二: 中方括号开头的前一条语句
var b = 3;
;[1, 3, 5].forEach(function (item) {
  console.log(item)
})
/*
 错误理解:
 a = b[5].forEach(function(e){
 console.log(e)
 })
4. 解决办法: 在行首加分号
5. 强有力的例子: vue.js库
6. 知乎热议: [https://www.zhihu.com/question/20298345](https://www.zhihu.com/question/20298345)
## 六.内存溢出与泄露
### 1. 内存溢出
一种程序运行出现的错误，当程序运行需要的内存超过了剩余的内存时, 就出抛出内存溢出的错误
内存溢出
var obj = {}
for (var i = 0; i < 100000; i++) {
  obj[i] = new Array(10000000)
}
console.log('------')
### 2. 内存泄露
占用的内存没有及时释放，内存泄露积累多了就容易导致内存溢出
常见的内存泄露:
闭包
意外的全局变量
function fn () {
  a = [] //不小心没有var定义
}
fn()
没有及时清理的计时器或回调函数
setInterval(function  () {
  console.log('----')
}, 1000)
## 七.对象得tostring与valueof
toString()函数和valueOf函数，这两个函数是Object类的对象生来就拥有的，而且他们还可以允许我们重写，toString()将对象转换为字符串，valueOf将对象转化为值
1、valueOf()偏向于运算，toString()偏向于显示
2、对象转换时，优先调用toString()
3、强转字符串的情况下，优先调用toString()方法；强转数字的情况下优先调用valueOf()
4、在有运算操作符的情况下valueOf()的优先级高于toString()，
注意：的是当调用valueOf()方法无法运算后还是会再调用toString()方法
