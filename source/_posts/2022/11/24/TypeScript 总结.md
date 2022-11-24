---
title: TypeScript 总结
tags: TypeScript
abbrlink: f2e91a06
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/ts.webp
---



# TypeScript 总结



## 1 . 基本概念

### 概念

- TS 是 JS 的超集 ， JS 所有的语法 TS 都支持，而且还额外添加了类型系统


- TS 属于静态类型（指编译后执行）的编程语言，JS 属于动态类型（边编译边执行）的编程语言



### 使用原因

- JS 类型系统存在一定历史缺陷，导致很多错误都是类型错误（type Error），且代码运行后才能发现，增加了 发现和查找 Bug 的时间，降低了开发效率
- 引入 TS 后，借助开发工具能在编写代码阶段就发现错误，降低和减少了 Bug 的出现，极大提升开发效率



### 使用方法

> 由于浏览器和 Node 不能直接识别 TS 代码，需使用编译工具将 TS 代码编译成 JS 代码

1. 下包： yarn add global typescript  /  npm i  typescript -g
2. 将文件由` .js  =>  .ts` ，并为文件中变量添加 **类型注解**
3. 运行命令 `tsc 文件名` ，将 TS 文件转换为 JS 文件
4. 执行转换后的 JS 文件

也可以使用 `ts-node` 包来简化使用步骤，详见官网

#### **类型注解** 

- 在变量后面加上 `：类型`  为变量添加类型，约束变量类型。如

```ts
let count:number = 10
let str:string = '你好'
```



## 2. 基础类型

> 包括 JS 原始类型和 TS 新增加的类型

- JS 原始类型 ： number/ string/ boolean/undefined/null/symbol 等
- TS 新增类型：联合类型、类型别名（自定义类型）、接口、元祖、自变量、枚举、void、any类型等



### 原始类型

```Ts
// 类型注解为 js 原始类型
let age:number = 18
let name:string = '张三'
let isLoading： boolean = true
// 数组写法一：
let numbers: number[] = [1, 3, 5]
// 写法二：
let strings: Array<string> = ['a', 'b', 'c']
// 函数写法 普通函数
function add（num1:number,num2:number):number{ return num1+num2 }
// 箭头函数
const add =(num1:number,num2:number):number => { return num1+num2 }
... 以此类推
```



### 联合类型

- 给变量添加多个类型的联合，表示值可以是其中任意一种类型，使用 `|` 分隔


```ts
// 数组成员可以是数字或字符串
let arr :(number|string)[]= [10,'张三']
```



### 类型别名（自定义类型）

- 给任意类型用 type 关键字起个别的自定义名字，简化代码、方便复用（推荐使用大写字母开头）


```ts
// 使用 type 关键字声明，名字首字母大写，可以搭配联合类型使用
type Arr = (number|string)[]
type AddFn =(num1:number,num2:number)=> number
let arr1:Arr= [20,'list']
let arr2:Arr= ['a',10]
const add: AddFn =(num1,num2) => num1+num2 
```



### 函数类型

- 指的是函数的参数和返回值的类型。参数需类型注解，否则会隐式转换成 any 类型，还可以设置可选参数；函数如果没有返回值或不需返回值可设置成 void 类型

```ts
// 参数需设置类型注解，否则会隐式转换为 any 类型
function hello( name: string): void { console.log(`hellow,${name}`)}

// 函数参数可传可不传，称为函数可选参数，在对应参数后面加上 ?（问号）且可选参数只能在参数的最后面，后面不能再有必选参数
function person(name:string,age:number,hobby?: string[]) :void{
     console.log('姓名:', name,'年龄:'age, '爱好:', hobby)
}
person('张三', 18)
```



### 对象类型

- TS 中对象类型指定义对象成员的类型，描述了对象结构

```ts
// 空对象  使用 {}来描述对象结构 属性采用属性名: 类型   方法名采用方法名():返回值的类型
let person: {} = {}
// 有属性的对象
let person1: { name: string } = { name: '同学' }
// 既有属性,又有方法
let person2: { name: string, fn(): void } = { name: '同学', fn() { } }

// 通过换行来分隔多个属性类型，可以去掉 `;`
let person3: {
  name: string
  sayHi(): void
} = {
  name: 'jack',
  sayHi() { }
}

// 直接使用 {} 的结构为对象添加类型,会降低代码可读性,更推荐使用类型别名为对象添加类型
// 对象的属性或方法也可以设置成可选状态
type Person = {
   name:string
   age?: number
   greet(name: string): void
}
   
let person: Person = {
    name: '张三'
    greet(name) {
    console.log(name)
  }
}
```



### 接口

- 当一个对象的类型被多次使用 , 可以使用接口 `interface ` 来描述对象类型，使之能复用

- 接口名称一般以大写 I 开头 ，只能用来定义对象类型
- 接口之间还可以通过关键字 `extends` 来继承

```tsx
 // 使用 interface 关键字声明接口，接口名首字母推荐 I 开头
interface Iperson {
 name: string
 sayHi(): void
}
let person : Iperson = { name: 'jack', sayHi(){}}

// 两个接口有相同的方法和属性 可以将公共的部分抽离出来,通过继承实现复用
interface Point2D { x: number; y: number }
interface Point3D { x: number; y: number; z: number }

// 二者都有 x,y 两个属性 可以使用继承方便使用
interface Point3D extends Point2D {
  z: number
}
```



###  元祖

> 是另一特殊类型数组

- 一些特定场景， 需要指定数组中元素的数量及对应下标的类型 ，就可以使用元组类型

```tsx
// 如经纬度的类型，直接使用数字数组不能指定数组元素的个数和对象下标的类型
let position: number[] = [116.2317, 39.5427]

// 使用元祖配合类型别名，使用 [] 包裹，里面放入对应成员的类型
type Position = [number,number] 
let position:Position = [116.2317, 39.5427]

```



### 字面量类型

- 任意 JS 字面量都可以作为字面量使用，一般配合联合类型一起使用

```ts
// str1 类型推导为 string  str2 类型是 'Hello TS' 是一个常量,值不能修改
let str1 = 'Hello TS'
const str2 = 'Hello TS'

// 配合类型别名和联合类型使用
type Direction = 'up' | 'down' | 'left' | 'right'

function changeDirection(direction: Direction) {
  console.log(direction)
}

// 调用函数可以使用点语法，会有类型提示。
// 只能四选一 相比于 string 类型,字面量类型更精确.严谨
changeDirection('up') 
```



### 枚举

- 用来表示一组明确的可选值列表，枚举功能类似于字面量 + 联合类型的功能
- 使用关键字 `enum` 定义枚举，首字母大写，多个值用逗号隔开，用 {} 包裹
- 枚举成员是有值的，可以设置初始值。枚举类型会被编译成 JS 代码
- 数字枚举值默认是从 0 开始自增的数值
- 字符串枚举没有自增长行为,每个成员必须设置初始值 

```ts
enum Direction { Up , Down , Left , Right}
function changeDirection(direction: Direction){ console.log (direction) }
// 调用函数时,需要传入枚举成员中任意一个 直接通过点语法访问枚举成员
changeDirection(Direction.Up)

// 数字枚举指定初始值，不指定会默认从0开始自增
enum Direction { Up=10 , Down=20 , Left=30 , Right=40}
// 字符串枚举必须指定初始值
enum Direction {
  Up = 'UP',
  Down = 'DOWN',
  Left = 'LEFT',
  Right = 'RIGHT'
}

// 其他类型在编译为js代码时会自动移除 , 但是枚举类型会被编译为 js 代码
var Direction;
(function (Direction) {
    Direction[Direction["Up"] = 0] = "Up";
    Direction[Direction["Down"] = 1] = "Down";
    Direction[Direction["Left"] = 2] = "Left";
    Direction[Direction["Right"] = 3] = "Right";
})(Direction || (Direction = {}));

```



### any类型

> 原则不推荐使用 any 类型，这会让 ts 失去类型保护的优势

- 当设置为 any 类型时，值可以进行任意操作而不会报错。
- 隐式转换： 当函数参数不声明类型或声明变量但不提供类型也不提供默认值时会隐式转换为 any 类型

``` tsx
let obj: any = { x: 0 }
// 声明为 any 后进行任意赋值操作都不会报错,也不会有提示
obj.bar = 100
obj()
const n: number = obj

```



### 类型断言

- TS 有类型推导机制，但其范围过于宽泛，会导致有些属性或方法无法使用，需指定更具体的类型，这时就可以用到类型断言
- 使用关键字 `as` 或 <> 来进行类型断言

```ts
// 类型推导为 HTMLElement 类型，该类型太过宽泛，无法操作 href 等 a 标签特有的属性或方法
const aLink1 = document.getElementById('link')
// 使用类型断言为更具体的类型，可以使用 as 或 <>，更推荐 as 方法
const aLink1 = document.getElementById('link') as HTMLAnchorElement
const aLink2 = <HTMLAnchorElement>document.getElementById('link') 
// 断言后即可使用 a 标签特有的属性和方法
aLink1.href = 'www.baidu.com'

```



### never 类型

- 表示一个不能取到的类型，或者说不存在值的类型
- 用于指定一个永远无法返回值的函数类型，如错误处理函数、无限循环函数等

```tsx
interface Foo { type: 'foo' }

interface Bar { type: 'bar' }

type All = Foo | Bar

// 定义一个函数, 正常情况下不会返回 never 类型, 但当同事修改了 All 类型时,如 type All = Foo | Bar | Baz
// 添加后忘了在函数内添加处理逻辑, 这时候就会返回 never 错误提示
function handleValue(val: All) {
    switch (val.type) {
        case 'foo':
            // 这里 val 被收窄为 Foo
            break
        case 'bar':
            // val 在这里是 Bar
            break
        default:
            // val 在这里是 never
            const exhaustiveCheck: never = val
            break
    }
}
```



### unknow 类型

- 是更加安全的 any 类型。
- 可以将任意值赋值给 unknown 但是不能调用属性和方法， 除非使用类型断言或类型收窄

```tsx
let value:unknown =10
// 可以任意赋值
// value = 10
// value = [10]

// 调用方法会报错
// console.log(value.length)

// 使用类型断言: 断言为 string , 即可调用方法
console.log((value as string).length)

// 使用类型收窄
if(typeof value === 'string'){
    console.log(value.length);
}
```



### typeof 类型查询

- 可以在 **类型上下文**中引用变量或属性的类型（类型查询）
- 只能用来查询变量或属性的类型，无法查询其他形式的类型（比如，函数调用的类型）

```tsx
let p = { x: 1, y: 2 }
function formatPoint(point: { x: number; y: number }) { }

// 使用 typeof 来获取变量 p 的类型,其结果与上等同,但是更简洁
function formatPoint1(point: typeof p) { }
// 类似于 key in 循环获取变量的键值类型

```



## 3. 高级类型

> TS 中常见高级类型有以下几种

- class 类型
- 兼容性类型
- 交叉类型
-  泛型和 keyof 类型
- 索引签名和索引查询类型
- 映射类型



### class 类型

- 在 TS 中，class 不仅是一种语法，而且还是一种类型
- 继承方式：`extends  ` 和  `implements`。
- 可见修饰符：1. public 公开的  2. protected 受保护的  3. private 私有的
- 只读修饰符： `readonly ` 表示只读，防止在构造函数之外对属性进行赋值

> 只读修饰符只能修饰属性不能修饰方法, 且在接口或者 {} 表示的对象类型中也可使用

```tsx
// 类实例初始化
class Person {
    // 实例初始化,此处可以省略类型注解,类型推导为 string
    name = 'zs';
    age: number;

    constructor(name: string, age: number) {
        // 构造函数形参需类型注解,否则会隐式推断为 any 类型
        // 实例初始化后才能通过 this 来访问, 否则会报错
        this.age = age;
        this.name = name;
    }

    // 方法的类型注解和函数一样
    hobby(e: string): void {
        console.log(this.name + '的爱好是' + e)
    }
}

// 1. extends 关键字继承父类( js 自带)
class Son extends Person {
    score: number
    constructor(name: string, age: number, score: number) {
        // 子类继承父类,想有自己的构造函数就必须调用 super 方法
        super(name, age)
        this.score = score;
    }
    learn(e: string): string {
        return `${this.name}爱学习${e},成绩是${this.score}`
    }
}
const p1 = new Son('王五', 22, 100)
p1.hobby('打豆豆')
p1.learn('数学')

// 2. implements 关键字实现接口( ts 提供的)
interface Iperson {
    move: (num: number) => void,
    work: string
}
// 使用 implements 让类实现接口意味着 类中必须要有接口中指定的所有属性和方法
class Person2 implements Iperson {
    name: string
    work: string
    constructor(name: string, work: string) {
        this.work = work
        this.name = name
    }

    move(num: number) {
        console.log(`${this.name}每天要走路${num}米上学,他的工作是${this.work}`);

    }
}

const p2 = new Person2('李白', '睡觉')
p2.move(1000)

// class 修饰符
class Animal {
    // public 表示公开的,公开的属性和方法可以被任意访问, 默认可见性,一般可省略
    public move(name: string) {
        console.log(name + '移动了')
        // 类 内部可访问
        this.run()
    }
    // protected 表示受保护的, 只能在声明的类和子类中访问,无法通过实例对象访问
    protected eat(name: string, food: string) {
        console.log(name + '今天吃了' + food)
    }
    // private 私有的,只在当前类中可见,子类和实例均不可见
    private run() {
        console.log('仅在 Animal 内部可使用 run');
    }
}
const dog = new Animal()
dog.move('dog')
// 实例中不可访问 受保护方法
// dog.eat('dog','meat')
// 私有属性实例无法访问
// dog.run()
```



### 类型兼容性

- TS 属于结构化类型系统，类型检查关注的是值所具有的形状。即如果两个对象拥有相同的形状，则它们属于同一类型

- TS 中类型兼容性分为 class 类型兼容性、接口兼容性和函数兼容性



#### class 类型兼容性

- 对象类型中，Y 的成员至少与 X 相同，则 x 兼容 y（成员多的可以赋值给少的）

```tsx
class A {
    X: number
    Y: number
}
class B {
    X: number
    Y: number
}
class C {
    X: number
    Y: number
    Z: number
}
// f1 是 A 类型的,值是 B 的实例,但是却不会报错,因为 ts 是结构化系统, 只负责检查结构是否相同
const f1: A = new B()
// f2 是 A 类型, 值是 C 的实例,也不会报错, 对于对象类型来说，y 的成员至少与 x 相同，则 x 兼容 y（成员多的可以赋值给少的）
// 在此处便是 C 的成员至少与 A 的成员相同, 所以 A 兼容 C 
const f2: A = new C()
// 反过来成员少的赋值给成员多的则会报错, 缺少成员多的属性
// const f3: C = new A()
```



#### 接口兼容性

- 接口兼容性类似于 class 兼容性,结构相同则接口也相互兼容 
- 接口还能与 class 之间相互兼容，成员多的可以赋值给成员少的, 少的兼容多的

```tsx
interface IPoint {
    x: number
    y: number
}
interface IPoint2D {
    x: number
    y: number
}
interface IPoint3D {
    x: number
    y: number
    z: number
}
// 接口兼容性类似于 class 兼容性,结构相同则接口也相互兼容 
let p1: IPoint = { x: 1, y: 2 }
let p2: IPoint2D = p1
let p3: IPoint3D = { x: 1, y: 2, z: 3 }
// 成员多的可以赋值给成员少的, 少的兼容多的
p2 = p3
// 成员少的不能赋值给多的, 多的不兼容少的
// p3 = p2

// 接口还能与 class 之间相互兼容
class Point3D {
    x: number
    y: number
    z: number
}
// 与 class 相互兼容, 成员多的可以赋值给成员少的, 少的兼容多的
let p4 = new Point3D()
let p5: IPoint3D = p4
let p6: IPoint = p4
// 成员少的不能赋值给多的, 多的不兼容少的
// p4 = p2

```



#### 函数兼容性

- 函数兼容性需考虑三方面，参数数量，参数类型和返回值类型。

```tsx
// 1. 参数数量: 参数数量少的可以赋值给多的, 与 class 和 接口 兼容性相反. 
type F1 = (a: string) => void
type F2 = (a: string, b: number) => void
let fun1: F1 = () => { }
let fun2: F2 = () => { }
// 参数多的不能赋值给成员少的
// let fun3:F1 = fun2
// 参数少的可以赋值给多的
let fun3: F2 = fun1

// 2. 参数类型: 相同位置的参数类型如果相同或兼容, 则 两个函数兼容

// 3. 返回值类型: 返回值是对象类型, 则成员多的可以赋值给成员少的, 少的兼容多的

```



### 交叉类型

- 用于组合多个类型为一个类型，常用于对象类型。功能类似于接口继承，使用关键字 & 连接

```tsx
interface IA {
    name: string;
}
interface IB {
    age: number;
}
// 使用 & 连接两个接口, 组合为一个新的类型,同时具备两个接口的所有属性类型
type C = IA & IB
// 类型别名 C 必须具备 name:string 和 age:number 类型
const person: C = {
    name: "John",
    age: 21
}

// 交叉组合同名类型, 组合后的类型为 联合类型| 
const fn: Fn = {
    fn(a: number | string) { }
}
```



### 泛型

>  常用于 函数, class , 接口等

- 指在保证类型安全的前提下, 让函数与其他类型一起工作,从而实现复用。

  

#### 泛型函数

- 在函数名后面用 <> 包裹 变量名 Type，函数参数和返回值均可使用该变量
- 函数参数和返回值类型取决于调用时传入的类型
- 由于泛型函数类型变量 Type 可以代表任意类型，会导致无法访问任何属性，需为该函数添加约束来收缩类型范围，称之为 **泛型约束**
- 类型变量还可以是多个，且类型变量相互之间还可以约束

泛型约束有两种方法：1. 指定更加具体的类型; 2. 添加更具体约束

```tsx
// 变量名 Type 是一种特殊类型的变量,专门用来处理类型，可以是任意合法的变量名
function id<Type>(value: Type): Type {
    return value
}
// 调用时传入什么类型, Type 接收到的就是什么类型
const num = id<number>(10)
const str = id<string>('你好')

function id1<Type>(value: Type): Type {
    // 此处访问 value 的 length 属性会报错,因为 Type 可以代表任意类型, 如 number, 而 number 是没有 length 属性的
    // 此时就需要为泛型添加约束来收缩类型范围
    // console.log(value.length);
    return value
}

// 添加泛型类型约束主要有两种方法: 1. 指定更加具体的类型; 2. 添加约束
// 1. 指定更加具体的类型
function id2<Type>(value: Type[]): Type[] {
    // 将 Type 的类型约束为 Type 类型的数组即可,而数组是一定有 length 属性的 
    console.log(value.length);
    return value
}

// 2. 添加约束
interface ILength {
    length: number
}

function id3<Type extends ILength>(value: Type): Type {
    // 通过接口继承, 为类型变量添加约束, 而继承的接口要求必须具有 length 属性, 因此此处可以确保传入的参数带有 length 属性
    console.log(value.length);
    return value
}

// 泛型的类型变量可以是多个,且类型变量相互之间还可以约束

function getProps<Type , Key extends keyof Type>(obj: Type, key: Key) {
    // 可以添加多个类型变量, 用逗号隔开, keyof 接收一个对象类型,生成其键的联合类型,可以是字符串或数字
    // 此处的 keyof Type 获取到的是传入的 person 对象的所有键的类型,即 'name'|'age' 类型
    // 变量 Key 受 Type 约束, 即 Key 只能是 Type 所有键中的一个, 或者说只能访问对象中存在的属性
    return obj[key]
}

const person = { name: 'John', age:18}
getProps(person, 'name')

```



#### 泛型接口

- 指的是接口也可以配合泛型来使用,增加其灵活性, 增强复用性
- 在接口名后用 <> 包裹类型变量, 就可以将该接口变成泛型接口, 接口内成员均可使用该变量
- 使用泛型接口时必须显式的指定具体的类型, 不再使用推导机制

```tsx
interface IFun<Type> {
    id: Type
    fn(): Type[]
    fun(value: Type): Type
}

// 使用泛型接口时必须显式的指定具体的类型, 不再使用推导机制
const obj: IFun<number> = {
    id: 18,
    fn() { return [10] },
    fun(val) { return val }
}

// 在 TS 中使用数组的方法,就是泛型接口,会根据数组的类型自动将类型变量设置为相对应类型
let arr = [1, 2, 3]
// 此处会根据 arr 的数组类型来自动将类型变量设置为 number
arr.forEach(()=>{})

let arr1 = ['1', '2', '3']
// 此处会根据 arr 的数组类型来自动将类型变量设置为 string
arr1.forEach(()=>{})

```



#### 泛型类

- 指的是 class 也可以配合泛型来使用,类似于泛型接口
- 在 class 类名后用 <> 包裹类型变量, 类成员均可使用该变量
- 使用时需显式的传入变量类型

```tsx
class Numer<Type> {
    defaultValue:Type
    add:(x:Type,y:Type)=> Type
}
// 使用时需显式的传入变量类型
const n = new Numer<number>()
n.defaultValue = 10

```



### 内置泛型工具类型

> TS 中内置了一些常用的工具类型, 来简化 TS 中的常见操作

常用的有以下几种：

-  Partial<Type>：用来创建一个类型, 将所有 Type 属性设置为可选

```tsx
interface IProps {
    id: string
    children: number[]
}

// 原有接口必须同时传入 id 和 children 属性以及对应类型
const obj: IProps = {
    id: '18',
    children: [10, 20]
}
// 使用 Partial 包裹 接口名称, 使其创建出新的类型,该类型结构与原接口结构完全一致, 但是属性都变为可选属性了
type PartialProps = Partial<IProps>
// 使用 Partial 工具类型改造后所创建的新类型, 结构一致但属性变为可选属性
const obj1: PartialProps = {
    id: 'aaa'
}
```

-  Readonly<Type>：用来创建一个类型, 将所有 Type 属性设置为 Readonly(只读)

```tsx
interface IProps {
    id: string
    children: number[]
}

type ReadonlyProps = Readonly<IProps>
// 使用 Readonly 工具类型改造后所创建的新类型 ReadonlyProps , 结构一致但属性变为只读,无法修改
const obj2: ReadonlyProps = {
    id: '18',
    children: [10, 20]
}
// 修改会报错,因为 id 是只读属性
// obj2.id = '20'
```

- Pick<Type, Keys>：从 Type 中选择一组属性来创建新类型

```tsx
interface IProps1 {
    id: string
    title: string
    children: number[]
}
// Pick 包含两个类型变量, Type 表示需要具体选择的接口, Keys 表示选择 Type 中的哪几个属性,多个可用联合类型
type PickProps = Pick<IProps1, 'id' | 'title'>
// 创建的新类型包含了原接口中所选择的哪几个属性
const obj3: PickProps = {
    id: 'bbb',
    title: '你好'
}
```

- Record<Keys,Type>：用来创建一个新类型, 属性键为 keys ,属性类型为 Type

```tsx
type RecordProps = Record<'a' | 'b' | 'c', string[]>
// 创建的新类型必须包含 a,b,c 三个属性,且三个属性类型都为 string[]
const obj4: RecordProps = {
    a: ['aaa'],
    b: ['bbb'],
    c: ['ccc']
}
```

- ReturnType<Type>: 获取/返回 函数返回值的类型, 常用于获取某个函数返回值的类型

```tsx
declare function fn(a: number, b: number): number
type Fn = typeof fn

// 获取 Fn 函数返回值类型 number
type Res = ReturnType<Fn>
```



### 索引签名类型

- 当对象或数组内部属性数量无法确认时，或者说对象、数组中可以出现任意多的属性时，就可以用到索引签名

- 在内部键使用 ` [key : 类型注解]` 来为对象或数组添加约束，表名成员可以是任意多个，但是成员类型都是约束一致的

```tsx
// key 是一个占位符,可以是任意合法变量名
interface IPerson {
    // PS: JS 中对象属性默认就是 string 类型的
    [key: string]: string
}
const onj: IPerson = {
    name: 'John',
    age: '18'
}

// 数组是一个特殊的对象, 键是数字类型, 值可以是任意类型, 且可以有多个, 故也可以使用索引签名类型
interface IArr<Type> {
    // 数组的 key 都是 number 类型
    [key: number]: Type
}
// 使用也需显式的传入类型
const arr:IArr<string> = ['a', 'b', 'c']
```



### 映射类型

- 基于旧类型创建新类型, 减少重复,提升开发效率
- 用 [] 包裹， key in Prop 来循环 Prop 中的键，设置相同的类型。类似于 for in 循环
- 注意： 映射类型只能在类型别名中使用, 无法在接口中使用

```tsx
// for example
// 联合类型
type PropKeys = 'x' | 'y' | 'z'
type Prop1 = { x: number; y: number; z: number }

// 通过映射来简化联合类型
type Prop2 = { [key in PropKeys]: number }

// 对象类型
type PropKeys1 = { x: number; y: number; z: number }

// 通过映射来简化对象类型
// keyof PropKeys 获取到对象类型的所有键的联合类型 'x'| 'y'| 'z', 然后 key in 循环该联合类型,就可以表示联合类型的每个键
type Prop3 = { [key in keyof PropKeys]: number }

// 之前所接触的内置泛型工具类型都是基于映射类型来实现的, 如 Partial<Type> 可选属性的实现
// 定义类型别名 Partial1 , 接收类型变量 T , keyof T 获取 T 中所有键的联合类型, 再通过 P in 来循环获取到每个键
// 将每个键类型通过 ?: 变为可选属性, 最后通过 T[P] 取出对象中键的值来赋值给对应的键. 最终所有键都和旧类型完全一致,只是属性类型变为可选的
type Partial1<T> = {
    [P in keyof T]?: T[P]
}
type Props = { a: number, b: string, c: string[], d: boolean }

type PartialProps = Partial1<Props>

```



## 4. 类型声明文件

> 主要分为两种： .ts  | .d.ts 文件

- .ts 文件：包含了类型信息和执行代码，可以被编译为 .js 文件，然后执行代码
- .d.ts 文件：仅包含类型声明文件，仅用于提供类型，不会生成 .js 文件



#### 第三方库声明文件

> 有些第三方库有自己的类型文件，使用时直接导入使用即可

当第三方库没有自己的类型文件时，需借助  DefinitelyTyped  包来下载对应的类型文件包

```tsx
// yarn add @types/react @types/lodash 
// 安装类型包后 TS 会自动加载
```



#### 项目创建类型文件

> 如果多个 .ts 文件中都用到同一个类型，此时可以创建 .d.ts 文件提供该类型，实现类型共享。

使用步骤：

- 1. 创建 index.d.ts 类型声明文件。
  2. 创建需要共享的类型，并使用 export 导出
  3. 在需要使用共享类型的 .ts 文件中，通过 import 导入即可

使用说明：

- TS 项目中也可以使用 .js 文件
- 在导入 .js 文件时，TS 会自动加载与 .js 同名的 .d.ts 文件，以提供类型声明。
- 对于 let、function 等具有双重含义(在 JS、TS 中都能用)，应该使用 declare 关键字，明确指定此处用于类型声明。
- 对于 type、interface 等这些明确就是 TS 类型的(只能在 TS 中使用的)，可以省略 declare 



## 5. 搭配 React 使用

> 分为两种情况：创建新的项目和在现有项目中添加 TS

#### 创建新项目

- 使用脚手架创建支持 TS 的新项目：`create-react-app my-app --template typescript`

创建后会发现有以下变化：

1. 多了 `tsconfig.json`  文件，属于 TS 配置文件，也可以通过 `tsc --init` 来生成
2. 使用 React 组件时，组件后缀名都变为 .tsx
3. 多了 `react-app-env.d.ts` 文件，是一个类型声明文件，用来指定项目类型

项目中 React 所对应方法的类型声明

> 包括 useEffect、useRef、useState、router、redux 等

- useEffect： 使用与 js 中使用并无差别,不涉及任何泛型参数

- useRef：给 useRef 指定对应 DOM 元素的类型, 然后使用非空类型断言 ! 或类型缩窄

```tsx
const App = () => {
	const inputRef = useRef<HTMLInputElement>(null)
	const click = () => {
		// 使用 useRef 时, 直接获取组件值会报错, inputRef.current 可能为 null
		// console.log(inputRef.current.value)
		// 解决方法: 1. 给 useRef 指定对应 DOM 元素的类型, 然后使用非空类型断言 !.
		console.log(inputRef.current!.value)
		// 方法2: 类型缩窄: 当 inputRef.current 不为 null 时再获取值
		// if (inputRef.current) {
		// 	console.log(inputRef.current.value)
		// }
	}
	return (
		<div className="App">
			<input type="text" ref={inputRef} />
			<button onClick={click}>非受控组件获取input的值</button>
		</div>
	)
}
```



- useState：接受一个泛型参数用于指定初始值的类型，推荐定义类型别名当做泛型参数

```tsx
// 使用 useState 只要提供了初始值, ts 会进行类型推断,因此此处的泛型参数可以省略
const [name, setName] = useState<string>('张三')

// 直接赋值初始值,通过类型推导机制来赋值类型
const [list, setList] = useState([{id:1, name:'zs'}])

// 更推荐写法: 定义类型别名,当做泛型类型
type Channel = { id: number, name: string}
const [list, setList] = useState<Channel[]>([])
```



- router：由于版本的变化，V6 版本中使用 useNavigate 代替了 useHistory, 传参无需指定泛型类型。V5 中使用 useLocation 和 useParams 时需补充类型变量, 使用 useHistory 时类型变量可传可不传

```tsx
import { Link, BrowserRouter as Router, Route, useLocation, Switch, useParams } from 'react-router-dom'
import { LoginState } from './type'
// 总结: 使用 useLocation 和 useParams 时需补充类型变量, 使用 useHistory 时类型变量可传可不传
const Home = () => {
	// const history = useHistory()
	// 注意点: HashRouter 通过 location 获取 state 参数为 undefined, 必须通过 BrowserRouter
	// const location = useLocation<{ a: number; b: string }>()

	// 首次加载页面 location.state 是 undefined , 所以此处需设置可选链操作符
	// 更好做法是单独声明类型文件, 传和接都用该类型
	const location = useLocation<LoginState>()

	const a = location.state?.a
	console.log(a, 888)

	const params = useParams<{ id: string }>()
	// 直接使用 params.id 会报错,需要传入泛型参数
	console.log(params.id, 777)
	return <div>首页</div>
}
const Login = () => {
	// v5 版本可以指定传参泛型类型
	// const history = useHistory< {a: number; b: string}>()
	// const history = useHistory<LoginState>()
	const location = useLocation()

	console.log(location)
	return (
		<div>
			登录页
			<br />
			<button
			// onClick={() => {
			// 	history.push('/home', { a: 1, b: '66666' })
			// }}
			></button>
		</div>
	)
}
const App = () => {
	return (
		<Router>
			<div>
				<Link to="/home/123">首页</Link>
				<br />
				{/* <Link to={{ pathname: 'login', state: { a: 1, b: 'zs' } }}>登录页</Link> */}
				{/* <Link to={{ pathname: 'login', state: { a: 1, b: 'zs' } }}>登录页</Link> */}
				<Link to="/login">登录页</Link>

				<hr />
				<Switch>
					{/* <Route path="/home" component={Home}></Route> */}

					<Route path="/home/:id" component={Home}></Route>

					<Route path="/login" component={Login}></Route>
				</Switch>
			</div>
		</Router>
	)
}

```



- redux ：分为三个方面的类型定义
  - RootState：在 store/index.tsx 中通过 `ReturnType` 获取所有  state 状态返回值，使用`useSelector`时给参数定义类型。
  - RootAction：给每个 action 文件定义类型，配合联合类型定义总的 `RootAction` 类型
  - RootThunkAction：使用 `thunk` 包后定义总的 action 类型，后续每个 action 方法都属于该类型
  - initState：给每个 reducer 文件定义初始值类型，函数参数类型就是定义的初始值类型和对应的 action 类型，返回值也是初始值类型

```tsx
// 定义总的 state 类型，后续每个调用 useSelector 方法的参数类型就是该类型
export type RootState = ReturnType<typeof store.getState>
// 使用 useSelector 获取 redux 数据，类型就是 RootState
const articleList = useSelector((state: RootState) => state.article)

// 每个 action 文件定义当前 action 的类型，使用类型别名配合联合类型导出
export type TodoAction = { type: 'TODO/GET_TODO' payload: ToDoItem[]}
 | { type: 'TODO/ADD_TODO' payload: ToDoItem  }

// 在 store/index.tsx 文件中，定义 RootAction ，当做 RootThunkAction 的泛型变量
export type RootAction = TodoAction | channelAction | articleAction
// thunk 包定义的 action 异步类型，包含四个参数，作为每个异步 action 的函数类型
export type RootThunkAction = ThunkAction<void, RootState, unknown, RootAction>

// RootThunkAction 作为异步 action 的函数类型
export const getTodoList = (): RootThunkAction => {
	return async (dispatch) => {
		const { data: res } = await axios.get('https://www.fastmock.site/mock/37d3b9f13a48d528a9339fbed1b81bd5/book/api/todos')
		dispatch({
			type: 'TODO/GET_TODO',
			payload: res.data,
		})
	}
}

// initState 给每个 reducer 定义初始值类型
export type initTodo = { id: number name: string isDone: boolean }

// 总结：单独的 acton 文件定义对应的 action 类型，包括 type 和 payload
// 单独的 reducer 文件定义对应的 initState 初始值类型，函数第一个参数和返回值类型就是该类型，action 类型就是对应的 acton 文件定义的类型
// 使用 ReturnType 泛型工具获取所有 state 类型，组件内使用 state 就需用到该类型
// 使用 RootThunkAction 设置给所有异步 action 函数类型
```



#### 在现有项目中添加 TS

大致步骤如下：

1. 下载对应类型包： yarn add typescript @types/node @types/react @types/react-dom @types/jest
2. 移除 `jsconfig.json` 
3. 创建 `path.tsconfig.json` 文件，将原来 `jsconfig.json` 文件中的内容拿过来
4. 创建 `tsconfig.json` ，进行相关设置

```json
{
  // 添加这一句
  "extends": "./path.tsconfig.json",
  "compilerOptions": {
    ...
  }
}
```

5. 创建并配置 react-app-env.d.ts 文件
6. 为项目中对应组件添加上类型注解
7. 重启项目

