---
title: js 高级总结
tags: js
abbrlink: e0f11382
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/js.webp
---



# js 高级总结

### 1. 面向对象

​	概念: 	是一种注重结果的思维方式   本质是对面向过程的封装

​	好处: 	便于维护, 方便复用 , 避免造成全局变量污染

​	三大特征 : 	

	1. 封装 : 将某个具体功能封装在对象中，对外部暴露指定的接口
	2. 继承: 一个对象拥有其他对象的属性和方法

```
// 继承又分为很多种, 常见的有原型链继承, 混合式继承, 替换原型继承 
```

3. 多态: 一个对象在不同情况下的多种状态

### 2. 构造函数

js 中内置了很多构造函数, 通过 new 关键字来调用

每一个构造函数都有  `prototype`  属性, 指向指向自身的原型对象

### 3. 原型对象

每一个函数被创建的时候，系统都会自动创建与之对应的对象,称之为原型对象

每一个原型对象都有 `constructor`  属性，指向自身对应的构造函数

### 4. 实例对象

通过 关键字 `new` 创建的对象都称之为该对象的实例对象

每个实例对象都有 `constructor` 属性, 指向该实例对象的原型对象

### 5. new 关键字

`new` 关键字的四个流程

	1.	创建空对象 {}
	2.	将  `this`  指向这个对象 this = {}
	3.	对象一 一赋值 this.age = age
	4.	返回这个对象 return this

### 6. 三者关系

```js
/*  构造函数、原型对象、实例对象三角关系
            1. prototype : 属于构造函数，指向原型对象
                * 作用：解决资源浪费+变量污染 
            2.__proto__ : 属于实例对象，指向原型对象
                * 作用： 可以让实例对象访问原型中的成员
            3.constructor: 属于原型对象，指向构造函数
                * 作用： 可以让实例对象 知道自己被哪一个构造函数创建的  */
```

### 7. 原型链

每个对象都有原型,而原型也是对象,也有自己的原型.以此类推, 形成链式结构，称为原型链。

对象访问原型链遵循就近原则,先访问自身,如果没有就访问他的上一级原型对象,如果一直没有就会报错. 其中属于构造函数实例化对象的成员变量，称之为实例成员,属于构造函数对象自身的成员变量，称之为静态成员

### 8. class 类与继承

- class 是 es6 新出的一个关键字, 声明一个类函数(相当于 es5 的构造函数 ),其将构造函数和原型都放在大括号内部, 阅读性高, 且必须通过 new 关键字调用

```
 class 构造函数名 {
 	// 每个 class 构造函数都有 constructor 相当于原有的构造函数
    constructor(name,age){
                  this.name = name
                  this.age = age
               }
   	 fn(){} // 相当于往原型中添加方法
   }
```

- extend 是 class 中用于继承的关键字,其原理是替换原型继承 

```js
//类函数
 class Person {
	//(1)构造函数
	 constructor(name, age) {
		  this.name = name
		  this.age = age
	  }
	//(2)原型对象
	eat() {
		console.log('我要吃饭')
		}
  }
class Student extends Person {
	 learn() {
		console.log('我爱学习')
	  }
  }
 console.log(new Student('班长', 38)) // {name:'班长',age:38} 原型上有 learn 方法, 原型的原型上有 eat 方法
```

- super 是当子类继承父类的时候, 想写属于自己的构造函数时就必须调用 super 方法

```js
 class Student extends Person {
        //想自己写构造函数
        constructor (name, age, score) {
          super(name,age)
          this.score = score
        }
        //原型
        learn () {
          console.log('我爱学习')
        }
      }
console.log(new Student('班长', 38, 100)) //  {name:'班长',age:38,score: 100} 原型上有 learn 方法, 原型的原型上有 eat 方法 
```

### 9. this 指向

###### this 通常指函数执行上下文对象, 其中 this 指向如下

- 全局函数( 普通函数 ) 调用: 指向 window
- 对象的方法调用: 指向调用的那个对象
- 构造函数的 this 指向 new 所创建的实例对象

箭头函数没有自己的 this , 通常指向上一级的作用域 ,像定时器等通常指向 window

###### 修改 this 指向三种方法

- call(修改后的this, 实参1, 实参2 ...): 适用于单个形参
- apply(修改后的 this, [实参1,实参2...]) , 适用于多个形参
- bind(修改后的 this , 实参1, 实参2...), 适用于不需要立即执行的函数(定时器函数,事件处理函数)

三者异同点: apply 传参方式不同,传的是一个数组,而 call/bind 传参是一个个的

bind 执行机制不同, call/apply 都是立即执行

### 10 . 检测数据类型

1.  typeof 检测

```js
console.log(typeof 1) // 'number'
console.log(typeof '1') // 'string'
console.log(typeof true) // 'boolean'
console.log(typeof undefined) // 'undefined'
console.log(typeof {}) // 'object'
console.log(typeof function () {}) // 'function'
// 无法检测 null 和 数组
console.log(typeof null) // 'object'
console.log(typeof []) // 'object'
```

2. Object.prototype.toString.call(数据)

```js
console.log( Object.prototype.toString.call(num) )//['object Number']
console.log( Object.prototype.toString.call(str) )//['object String']
console.log( Object.prototype.toString.call(bol) )//['object Boolean']
console.log( Object.prototype.toString.call(und) )//['object Undefined']
console.log( Object.prototype.toString.call(nul) )//['object Null']
console.log( Object.prototype.toString.call(arr) )//['object Array']
console.log( Object.prototype.toString.call(fn) )//['object Function']
console.log( Object.prototype.toString.call(obj) )//['object Object']
```

2. instanceof :  检测右边构造函数的原型在不在左边实例对象的 原型链中

```js
// 不能检测基本数据类型
console.log(arr instanceof Array) //true
console.log(arr instanceof Object) //true
```

### 11. 深浅拷贝

js 中进行直接赋值操作时,如果是值类型,修改赋值后的数据原数据也不会改变,但是如果是引用类型赋值, 其实际赋值的是栈中的地址, 修改拷贝后数据原数据也会改变,称之为浅拷贝

浅拷贝: 拷贝的是栈中的地址, 修改浅拷贝后的数据,原数据也会改变

深拷贝: 指的是引用数据类型进行了深度拷贝(堆中的数据),修改拷贝后的数据对原数据没有影响.

常见的深拷贝方式: 

- 转 JSON: 	JSON.parse(JSON.stringify())
- 递归赋值

### 12. 递归函数

指在函数内部自己调用自己,如使用递归,必须要有结束条件,否则会导致死循环

### 13. let / const/ var 的区别

相同点: 三者都是声明变量的关键字,其中 let 和 const 都有块级作用域

不同点: var 没有块级作用域的概念,且存在变量提升, 可以先使用后声明

const 声明的是常量, 不能修改, let 和 var 声明的变量可以被修改

### 14. 解构赋值

解构赋值就是变量赋值的简写形式,基本语法如下

```js
//  取出对象的值 赋值给 变量
//	let { name,age,sex } = obj
// 取出变量的值 赋值给 对象
//  let obj = { name,age,sex }
// 在对象中, 键和值相同可以简写成一个 {name:name} => {name}
// 在对象中, 函数可以省略 function ,直接写函数名(){}
// 可以修改解构的变量名	let { name:res,age,sex } = obj
// 还可以设置变量默认值  let { name, age = 18 , sex: gender, hobby } = obj
// 函数的参数也可以设置解构和默认值
```

### 15. 箭头函数

本质就是 function(){} 的简写,  ()=>{}

当只有一个形参时括号可以省略, 当函数体只有一行，则可以省略大括号，同时必须要省略return

箭头函数本身是没有this的，本质是通过作用域链，访问上一级的this

### 16. 扩展运算符

简写形式 ...obj , 即对象或数组的遍历简写, 可用于数组/对象的合并,也可以用于伪数组转数组

```js
let oDivs = document.getElementsByTagName('div');//获取页面中所有的div标签
oDivs = [...oDivs];// 将伪数组转换为真正的数组
let arr= [10,20,30]
let arr1= [40,50,60]
let arr2= [...arr,...arr1] // [10,20,30,40,50,60]
```

### 17. 伪数组转数组

伪数组,即有数组的下标和长度,但是不能使用数组的方法, 本质是一个对象

```js
let obj = {
	0: 88,
	1: 90,
	2: 100,
	3: 66,
	4: 50,
	length: 5,
}
// 方法一: 遍历添加
let arr = []
for (let key in obj) {
	arr.push(obj[key])
}
console.log(Array.isArray(arr))// true
// 方法二: 使用 ES6 Array.from
let arr1 = Array.from(obj)
console.log(Array.isArray(arr1))// true
// 方法三:使用 apply 方法修改 this 指向
let arr2 = []
arr2.push.apply(arr2, obj)
console.log(Array.isArray(arr2))// true
// 方法三: 使用 call 修改 this 指向
// let arr3 = Array.prototype.slice.call(obj)
let arr3 = [].slice.call(obj)
console.log(Array.isArray(arr3)) // true
```

### 18.Set 和 Map

均是 ES6 新出的数据类型 

- Set 是唯一值的集合,类似于一个无重复值的数组。可以用于数组去重

```js
const a = "a" // 创建变量
const letters = new Set() // 创建新的 Set 
letters.add(a) // 向 Set 添加新元素。
letters.delete(a) // 删除由其值指定的元素。
letters.has(a) // 判断值是否存在,存在返回 true
let arr = [10,20,30,10,20,30]
let newArr = [...new Set(arr)] //[10,20,30] // 数组去重
```

- Map 对象存有键值对，其中的键可以是任何数据类型。类似于对象

```js
const apples = {name: 'Apples'} // 可以创建对象作为键
let fruits  = new Map() // 创建新的 Map 对象。
fruits.set(apples, 500)	// 为 Map 对象中的键设置值
fruits.get(apples)	// 500 获取 Map 对象中键的值。
```

### 19. 防抖和节流

- 防抖: 用户一段时间内频繁触发某事件, 只会执行最后一次, 例如搜索框输入事件

- 节流: 一段时间内多次触发只会执行一次,必须等这次执行完毕后才会执行下一次

  相同点:	都是为了优化函数执行频次, 优化网页性能

  不同点:	防抖是优化用户主动触发的事件, 多次触发只执行最后一次

  ​				节流是优化事件本身执行的频率,间隔时间内只执行一次