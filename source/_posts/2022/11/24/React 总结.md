---
title: React 总结
tags: React
abbrlink: 4f68be61
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/react.webp
---



# React 总结



## 1. 基本概念

React 是用于构建用户界面的 js 库, react 全家桶才是框架.全家桶包括

- react : 核心库
- react-dom : dom 操作
- react-router : 路由管理
- redux: 集中状态管理

全球范围内 react 使用范围最广,使用人数最多,下载量也最大.附上三大框架下载趋势

[react, vue, angular的下载趋势](https://www.npmtrends.com/angular-vs-react-vs-vue)

react 特点: 

- 声明式: 只需要描述 UI 的样子,就跟写 html 一样. 用类似 html 的语法定义页面,通过数据驱动视图, 高效更新数据并渲染 DOM
- 组件化: 组件用于表示页面中的部分内容, 是 react 中最重要的内容,组合复用多个组件就能实现完整页面功能
- 使用广: 使用 react/react-dom 可以开发 web 应用, 使用 react/react-native 可以开发移动端应用,还可以使用 react/react360 开发 VR 等.一次学习多次使用.众多大厂都在用



## 2. React 脚手架

1. 官方开发的脚手架 `create-react-app`, 先安装脚手架工具包, 再使用脚手架命令来创建项目

```js
// 1. 下包 npm i -g create-react-app
// 2. 使用脚手架命令创建项目 create-react-app 项目名称
```

2. 使用 npm v5.2 版本新添加的命令来创建.

```js
// 1. 创建命令 npx create-react-app 项目名称
// 2. npx create-react-app 是内置的固定命令,不能修改,项目名称可以自定义
```



## 3. 项目目录

使用脚手架创建项目后,会自动生成 src\public\node_modules 等目录,其中 src 就是进行项目开发的目录, src/index 就是项目的入口文件

```js
// 导入 react 和 react-dom
import React from 'react'
import ReactDOM from 'react-dom'
// 导入根组件
import App from './App'
// 创建元素 这是 react 18 版本更新后的新写法
const root = ReactDOM.createRoot(document.getElementById('root'))
root.render( <App />) // 渲染react元素

// 创建元素 这是 react 18 版本前的写法
ReactDOM.render(<App />, document.getElementById('root'))
```



## 4. JSX 语法

 react 渲染是 React.createElement 格式. 由于这种格式太繁琐不优雅就出现了更好用的 jsx 语法

```js
// React.createElement('标签名',{标签上的属性1：值1},子元素1，子元素2)
React.createElement('h1', {id:'box'}, 'hello react')
```

JSX 是 JavaScript XML 的缩写,可以在 JS 中书写 XML 结构, react 用它来创建 UI(HTML) 结构.

```js
// 注意: jsx 不是标准的 js 语法,是 js 语法扩展.脚手架内置了 @babel/plugin-transform-react-jsx 包来解析该语法
const root = (<div className={ style.root }>
                <h1 className="title">登录</h1>
              </div>)
// 该语法采用类似于 html 的语法,降低了学习门槛,使得页面结构更清晰直观.
```

使用 JSX 语法有几点需要注意

- jsx 语法必须要有一个根节点
- 属性名如需使用 js 关键字需要使用代替语法. 如 类名 class 要使用 className 代替, label 中的 for 需使用 htmlFor 代替
- 单标签需闭合, 双标签也可简写成单标签+闭合标签
- 如有换行或多行 html 结构建议使用 ( ) 包裹
- react 16.8 版本前必须先引入 react 才能使用 jsx 语法,原因是 jsx 本质是个语法糖

```js
import React from 'react'
function Login () {
  return <h1>Hello World</h1>
}
// 会被转换成如下所示, 其结果需要用到 react 所以必须导入
function Login () {
  return React.createElement('h1',null, 'Hello World')
}
// react 脚手架升级版本后内部做了一些处理,所以不需要再导入 react 了
```

### 1.JSX 表达式

​	使用  `{ JS 表达式}`  可以执行大括号内部的代码并输出结果. 

```js
// 1. 可以写属性值
const logo = 'https://create-react-app.dev/img/logo.svg'
<img width="80" src={logo} />
// 2. 可以写内容
const name = '小芳'
<div>{name}</div>
// 3. 可以写表达式,加减乘除和三元表达式等
const flag = 0
<div>{flag ? '达成' : '没有达成'}</div>	<div>{ 1+2 }</div> 
// 4. 可以写注释
{/* 所有子节点，都使用普通的 CSS 类名 */}
// 5. 不能写对象或 js 语句,包括 if/switch-case 等
 <p>{{ a: 1 }}</p> 		 <p>{var a =1 }</p>
```

### 2. JSX 列表渲染

可以在 JSX 中使用数组的 map 方法来生成列表结构, 可以重复生成相同的 HTML 结构

```jsx
const skills = [
  { id: 1, name: 'html' }, 
  { id: 2, name: 'css' }, 
  { id: 3, name: 'js' }
]
 <ul>
      // 在渲染的同时需加上 key
    {skills.map(item => <li>技能{item.id}: {item.name}</li>)}
  </ul>
```

在 React 内部渲染结构的同时,为了性能得到优化,必须同时加上 key 值, key 值不可以重复

### 3. JSX 样式处理

在 JSX 中写行内样式时,需使用双大括号包裹,外面的大括号表示里面的内容是一个表达式,里面的大括号表示是一个对象. 

```js
// 行内样式格式
<dom元素 style={ {css属性1：值1,css属性2：值2} }></dom元素>
// 属性名不能有- , 需使用小驼峰格式命名 
// 属性值是字符串, 如果单位是 px 可以简写数值
// 示例
<h1 style={ {color: 'red', width: 200, backgroundColor: 'black'} }>
  我是黑底红字的h1
</h1>
```



## 5. eslint 配置

### 目标

在 react 项目中配置 eslint ，并启用保存自动格式化功能

```js
// 1. 下包安装 eslint.  npm i eslint  -D
// 2. 在根路径运行命令 npx eslint --init,按照提示安装插件,会自动生成 eslint 配置文件
// 3. 设置 vscode 的自动保存格式化 设置=> eslint => 勾选 always show status
// 4. 根路径下补充配置文件 .vscode/setting.json 内容如下
{
  "eslint.run": "onType",
  "eslint.options": {
    "extensions": [".js", ".vue", ".jsx", ".tsx"]
  },
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

eslint 并不能深入到 jsx 代码中完成格式化, 需要额外的工具  ` prettier-now`, 该插件是 prettier 插件的分支, 允许更多配置项. 在 `.vscode/setting.json` 中配置

```json
{
  "eslint.run": "onType",
  "eslint.options": {
    "extensions": [".js", ".vue", ".jsx", ".tsx"]
  },
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  // 编辑器设置 - 保存时做格式化
  "editor.formatOnSave": true,
  // 编辑器设置 - 默认采用prettier-now做格式化
  // 如果使用的是prettier，这的设置应该是 esbenp.prettier-vscode
  "editor.defaultFormatter":"remimarsal.prettier-now",
  // 控制缩进
  "prettier.useTabs": false, // 缩进不使用tab，使用空格 
  "prettier.tabWidth": 2, // 缩进字节数 
  // 函数声明时小括号前后要加空格
  // 如果你使用prettier这一项是不能做选择的，导致和eslint默认配置的冲突
  // 可以在百度中搜到很多的记录： https://www.baidu.com/s?wd=prettier%20%E5%87%BD%E6%95%B0%E7%A9%BA%E6%A0%BC
  "prettier.spaceBeforeFunctionParen": true,
  // react的jsx让>与结束标签同行
  "prettier.jsxBracketSameLine": true,
  "prettier.bracketSpacing": false, // 去掉数组内部前后的空格
  "prettier.semi": false, // 不要给语句加;
  "prettier.singleQuote": true, // 采用单引号
  "prettier.trailingComma": "none", // 不要尾随逗号,
  "prettier.printWidth": 80, // 每行超过80列就换行
  // 在.js中，写div按下tab就可以自动补全，而不需要写<div再补全
  "emmet.includeLanguages": {
    "javascript": "javascriptreact"
  }
}
```

eslint 还可以使用插件帮助检查 Hooks 的使用是否符合规则

```
// 安装插件 npm install eslint-plugin-react-hooks -D
// 配置规则: 在 eslint 配置文件规则中添加两条
{
  "plugins": [
    // ...
    "react-hooks"
  ],
  "rules": {
    // ...
    // 检查 Hooks 的使用规则
    "react-hooks/rules-of-hooks": "error", 
    // 检查依赖项的声明
    "react-hooks/exhaustive-deps": "warn"
  }
}
```



## 6. 大厂 UI 库

1. DIDI
   1. vue移动端 [Cube UI](https://didi.github.io/cube-ui/#/zh-CN)
2. JD
   1. vue移动端: [NUTUI](https://nutui.jd.com/#/index)
3. 饿了么
   1. vuePC端 [elementUI](https://element.eleme.cn/#/zh-CN)
4. 蚂蚁
   1. react PC端[antdesign](https://ant.design/docs/react/introduce-cn)
   2. vue PC端 [vue](https://antdv.com/docs/vue/introduce-cn/)
5. 字节
   1. react PC端 [arco](https://arco.design/react/docs/start)
   2. vue3 PC端[arco](https://arco.design/vue/docs/start)

---



## 7. react 组件

组件就是对特定功能的封装,主要用来对 UI 进行拆分.其特点是可复用，独立，可组合.

可以分类为以下几种

- 基础组件: 指 `input` , `button` 等这种基础的标签组件,以及 UI 库封装过的通用 UI 组件
- 业务组件: 由基础组件组合成的业务抽象化 UI , 如包含了某公司所有部门信息的下拉框
- 区块组件: 由基础组件和业务组件组合而成的 UI 块
- 页面组件: 展示给用户的最终页面,一般一个页面组件对应一个路由规则

在 React  中组件主要分为两类: 函数式组件和类组件,  **使用组件 **可以使用单双标签包裹组件对象

### 1. 函数式组件

函数式组件即使用 JS 中的函数所创建的组件,一般是一个单独的 JS 文件 

```js
// 定义一个函数式组件 函数名首字符必须大写, react 据此区分组件和普通的 html 且必须有返回值
const Com1 = () => {
  return <div>第一个函数式组件</div>
}
// 默认导出
export default Com1
```

### 2. 类组件

类组件即使用 JS ES6 语法中的 `class` 所创建的组件,使用比较繁琐

```js
// 定义一个类组件类组件首字母也必须大写 extends 是关键字,用来实现类的继承.
// 类组件应该继承 React.Component 父类从而使用父类中的属性和方法
// 类组件必须提供 render 方法, render 方法必须有返回值且在组件创建时会执行一次
class Com2 extends React.Component {
  render () {
    return <div>第一个类组件</div>
  }
}
// 默认导出
export default Com2
```

### 3.组件状态

状态(state) 是用来描述失误在某一时刻的数据,例如 18岁时的身高,8月18日的电费等,在 react 中表示组件的私有数据. 其特点就是**状态能被修改**,修改后视图也会变化,其作用有两点

- 保存数据. 如要循环生成一份歌曲列表，要提前准备好歌曲数据
- 为后续更新视图打下基础。如果用户操作了状态内容，视图会自动更新（由react库决定的）

组件分为有状态组件和无状态组件. 在 react 16.8 版本中引入了 **Hooks**,从而函数式组件也能定义状态了

- 有状态组件：能定义state的组件。类组件就是有状态组件。
- 无状态组件：不能定义state的组件。函数组件又叫做 无状态组件

```js
import React from "react";
export default class Hello extends React.Component {
  // 类组件中有两种方式定义状态,较方便和常用的就是第一种
  // 方式1 state 定义状态
  state = {
    list: [{ id: 1, name: "明天会更好" },{ id: 2, name: "难忘今宵" }],
    isLoading: true
  };
  // 方式2 构造函数定义状态
  constructor() {
    super() // 在构造函数内部使用this，必须提前调用super
    this.state = {
      list: [{ id: 1, name: "明天会更好" },{ id: 2, name: "难忘今宵" }],
      isLoading: true
    }
  }
 // 视图中使用 state 定义的数据, 通过 this.state 来使用
  render() {
    return ( 
      <div>
        <h1>歌单-{this.state.count}</h1>
        <ul>
          {this.state.list.map((item) => (
            <li key={item.id}>{item.name}</li>
          ))}
        </ul>
        <div>{this.state.isLoading ? "正在加载" : "加载完成"}</div>
      </div>
    );
  }
}
```

### 4. 状态更新

类组件中通过 setState 方法来修改 state 数据,修改后 state 变化, 视图会自动更新

```jsx
state = { count: 0 };
// 通过 setState 方法修改
this.setState({ count: this.state.count++ })
// 注意: react 核心理念就是状态不可改变, 而是创建新的状态数据去覆盖原有的数据
this.state.count = 100 // 无效设置,会报错
```

setState 方法其特点是调用之后，并不会立即去修改 state 的值，也不会立即去更新 dom , 而是会放在更新队列里, 等待多次调用完毕后统一触发一次 render() ,此时表现的像是 **异步执行** 一样.

使用不当会造成死循环,如在 componentDidUpdate，render 中调用 setState() 会导致死循环

```jsx
export default class App extends Component {
  state = { n: 1 }
  hClick = () => {
    this.setState({n: 100})
    console.log(this.state.n) // 会打印 1
    console.log(document.getElementById('span').innerHTML) // 会打印 1
  }
  render () {
    return (
      <div>
        <button onClick={this.hClick}>click</button>
        <span id="span"> {this.state.n}</span>
      </div>
    )
  }
}
```

setState 方法不仅可以是一个对象, 还可以是一个回调函数, 其作用是当 setState 生效（ state 更新，页面 ui 更新）之后，会调用该回调函数

```jsx
this.setState((上一状态) => {
  return 新状态
}[,回调函数])
// 更推荐语法: 传入一个回调函数,参数就是上一次 setState 结果 适用于需要调用多次 setState
state = { count: 1 }
this.setState((preState) => {
    console.log(preState,222)  // 1  先打印 111 再打印 222
    return { count: preState.count + 1 }
})
console.log(this.state.count,111) // 1 
// 在状态更新（页面完成重新渲染）后立即执行某个操作 就会用到完整语法
this.setState(
	(state) => ({ count: state.count + 100 }),
	() => {
        // 此处页面更新后触发,可以获取到更新后的数据
      console.log('这个回调函数会在状态更新后立即执行', this.state.count) // 101
    }
)
```





## 8. 事件绑定

React 中用驼峰命名法定义事件名,如 onMouseEnter、onFocus、 onClick ... 

```js
// 格式 <元素 事件名1={ 事件处理函数1 } 事件名2={ 事件处理函数2 }  ></元素>
// 在类组件中定义方法, 通过 this.方法名来访问,也可以直接写箭头函数
export default class Hello extends React.Component {
  fn() {
    console.log('mouseEnter事件')
  }
  render() {
    return (
      <div
        onClick={() => console.log('click事件')}
        onMouseEnter={this.fn} >
        能处理鼠标进入或者点击事件
      </div>
    )
  }
}
```



## 9. 事件对象

React 中通过事件处理函数的形参来获取事件对象

```jsx
// 此处的 e 就表示事件对象
handleClick(e)=> {
    e.preventDefault()
    console.log('单击事件触发了', e)
}
render() {
  return (
  	<div>
      <button onClick={(e)=>{console.log('按钮点击了', e)}}>按钮</button>
      <a href="http://baidu.com/" onClick={this.handleClick}>百度</a>
  	</div>
  	)  
  }
}
```



## 10. this 指向

在函数式组件中不存在 this 指向问题,而只在类组件中存在着 this 指向问题

- 在 render 函数中 this 指向的是当前的 react 组件.
- 在自定义的方法中 this 原本指向 window, 由于 react 在 class 内部默认开启 JS 严格模式,故此处的 this 会指向 undefined

想解决 this 指向问题有三种方式,第一种最方便常用

- 1. 使用 class 的实例方法,即实例方法使用箭头函数

```jsx
class App extends React.Component {
  handleClick = () => {
  // 此处的 this 指向外部作用域, 即组件对象
    console.log(this)
  }
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>点我</button>
      </div>
    )
  }
}
```

- 2. 使用 bind 修改 this 指向

```jsx
class App extends React.Component {
  handleClick() {
     // 页面渲染后绑定了 this ,此处的 this 指向该组件对象
    console.log(this)
  }
 // 组件创建 this 指向的是该组件
 console.log(this)
  render() {
    return (
      <div>
        <button onClick={this.handleClick.bind(this)}>点我</button>
      </div>
    )
  }
}
```

- 3. 在外层补充箭头函数

```jsx
class App extends React.Component {
  handleClick() {
   // 此处的 this 指向外层作用域的 this , 其需要额外包裹箭头函数,结构不美观
    console.log(this)}
  render() {
    return (
      <div>
        <button onClick={() => {this.handleClick()}}>点我</button>
      </div>
    )
  }
}
```



## 11. 获取表单元素的值

在 react 中有两种方法获取表单元素的值

- 直接找到表单元素进行 dom 操作,此方法称之为 **非受控组件**
- 将表单元素的值与 state 进行绑定, 将用户的修改同步到 state 中,称之为 **受控组件**

### 1. 非受控组件-ref

借助于 ref 获取 DOM 元素，使用原生 DOM 的方式来获取表单元素的值,使用不多

```
// 1. 导入方法 import { createRef } from 'react'
// 2. 创建方法并引用 const refDom = createRef()
// 3. 绑定给表单元素的 ref 属性. <input ref={ refDom }/>
// 4. 通过 refDom.current.value 来获取值. console.log(refDom.current.value)
```

### 2. 受控组件

正常情况下，表单元素 input 可任意输入内容，可以理解为 input 自己维护它的状态（value）

在 state 中定义状态并绑定给表单元素的 value 值, 同时绑定 onChange 事件修改状态值

```jsx
// 使用受控组件处理表单元素，状态的值就是表单元素的值.操作表单元素的值，只操作对应的状态即可
// 1. 定义状态并绑定给表单元素
 state = {
    msg: 'hello react'
  }
  handleChange = (e) => {
    this.setState({ msg: e.target.value })
  }
  render() {
    return (
      <div>
        <p>  {/* 2. 表单元素绑定事件 */}
          	<input type="text" value={this.state.msg} 
          	onChange={this.handleChange}/>
        </p>
      <div>
    )
  }
// 不同类型的表单元素进行受控处理时的格式是不同的. 多表单元素还可以优化
// 1. 文本框、文本域、下拉框：value属性 + onChange事件
// 2. 复选框和单选按钮: checked属性 + onChange事件
  handleChange = e => {
    let {name, type, value, checked} = e.target
    console.log(name, type, value, checked)
    // 此处判断表单元素的类型,同时表单元素需要设置 name 属性为绑定的状态属性名
    value = type === 'checkbox' ? checked : value
    this.setState({ [name]: value })
  }
姓名：<input type="text" name="username" value={this.state.username} onChange={this.handleChange}/>
```



## 12. 组件通讯

实际开发中,组件是一个单独的 JS 文件，其状态是私有的,只能在内部使用。想共享某些数据就需要有组件通讯

一般有三种方式:  父子组件之间	兄弟组件之间	跨组件层级

### 1. 父子通讯

```jsx
// 父组件中传递数据 =>  <子组件 自定义属性1={值1} 自定义属性2={值2} .... />
// 子组件接收数据： 函数式组件需要通过形参来获取 类组件需要通过 this.props 来获取
function 子组件(props) {
  console.log('从父组件中传入的自定义属性被收集在对象:', props)
}
class 子组件 extends Component {
  console.log('从父组件中传入的自定义属性被收集在对象:', this.props)
}, 
// props 的特点: 可以传递任意数据, 传递的数据是只读的, 即单向数据流,无法直接修改
// 单向数据流是指 父向子传值的流向是单向的 父组件传递的数据更新时子组件会自动接收到最新数据
// props 中还有 children 属性, 表示该组件的子节点, 即该组件所包裹的内容
// 只要包裹了内容, props 就有该属性
// 例如: 
<Hello>我是子节点</Hello>
function Hello(props) {
  console.log(props.children) // 我是子节点
}

// 子传父: 利用父组件提供的回调函数来传递要修改的数据,回调函数的形参接收要修改的数据
// 例如:
class Parent extends React.Component {
    state: { num: 100 }
	// 自定义函数, 当做值传递给子组件,并通过形参接收子组件要修改的数据
    fn = (num) => {
        console.log('接收到子组件数据', num)
    }
    render() {
        return (
            <div>
            	子组件：<Child f={this.fn} />
            </div>
        )
    }
}
// 子组件调用父组件传过来的函数,并传入要修改的值
class Child extends React.Component {
    handleClick = () => {
      // 调用父组件传入的props，并传入参数
    	this.props.fn(100)
    }
    return (
    	<button onClick={this.handleClick}>点我，给父组件传递数据</button>
    )

```

### 2. 兄弟组件通讯

使用**状态提升**, 实现兄弟组件之间的组件通讯

```jsx
// 1. 将共享状态提升到最近的公共父组件中,由父组件管理状态,要通讯的子组件只需要接收状态即可
// 例如:
import Son1 from './Son1'
import Son2 from './Son2'
class App extends Component {
  // 1. 状态提升到父组件 
  state = { msg: 'jack'}
 // 提供方法修改状态 Son1 修改 Son2 的数据
  changeMsg = (msg) => {
    this.setState({ msg })
  }
  render() {
    return (
      <div>
        <h1>我是App组件</h1>
        <Son1 say={this.changeMsg} />
        {/* 2. 把状态给子组件显示 */}
        <Son2 msg={this.state.msg} />
      </div>
    )
  }

export default class Son1 extends Component {
  say = () => {
    this.props.say('rose')
  }
  render() {
    return (
      <div>
        <h3>我是 Son1 组件</h3>
        <button onClick={this.say}>说</button>
      </div>
    )
  }

export default class Son2 extends Component {
  render() {
    return (
      <div>
        <h3>我是 Son2 组件-{this.props.msg}</h3>
      </div>
    )
  }
}
```

### 3. 跨组件通讯

使用 **`Context`** 实现跨级组件通讯, 主要分为三个步骤

- 1. 导入并调用 createContext 方法，从结果中解构出 Provider, Consumer 组件

```jsx
// 可以单独建立 context.js 文件 也可以直接在组件内部创建并导出 Consumer 组件
import { createContext } from 'react'
const { Provider, Consumer } = createContext()
export { Consumer, Provider }
// 使用 Provider 包裹根组件, 并通过 value 传递数据
render () {
    return (
      <Provider value={{ num: this.state.num }}>
        <div>
          <Parent />
          <Uncle />
        </div>
      </Provider>
    )
  }
// 在任意后代组件中，使用导出的 Consumer 组件包裹整个组件, 并通过形参接收
import { Consumer } from './context'
return (
	<Consumer>
  	{（data） => {
      	// 这里的形参data 就会自动接收Provider中传入的数据
      	return <组件的内容>
    	}}
  </Consumer>
)
```

- 2. 使用 **Provider** 组件**包裹根组件**，并通过 **value** 属性提供要共享的数据
- 3. 在任意后代组件中，使用导出的 Consumer 组件包裹整个组件,形参接收数据



## 13. 传值 props 校验

对于组件来说，props 是外部传入的，无法保证组件使用者传入什么格式的数据.

如果传入的数据格式不对，可能会导致组件内部报错。**组件的使用者不能很明确的知道错误的原因。**故可以通过 props 校验来约定 prpos 的格式和数据类型,不满足条件就报错,增加组件健壮性

```jsx
// 1. 导入组件 这个包在使用脚手架创建项目时就自带了，无须额外安装
import PropTypes from 'prop-types'
// 2. 定义方法约定类型
组件名.propTypes = {
    // 约定类型, 如果类型不对，则报出明确错误，便于分析错误原因
    // 常见类型如 array、bool、func、number、object、string
    属性名: PropTypes.array // 约束类型
	// 必填项：isRequired
    属性名: PropTypes.array.isRequired
    // 特定结构的对象：shape( { } )
    属性名: PropTypes.shape({
        color: PropTypes.string,
        fontSize: PropTypes.number
	})
}
// 还可以设置 props 默认值,在未传入 props 时生效,
组件名.defaultProps = { pageSize: 10 }
// 也可以使用解构赋值设置默认值
const { pageSize = 10} = this.props
<div> 此处展示props的默认值：{props.pageSize} </div>
```



## 14. 组件生命周期

> 只有类组件才有生命周期

组件的生命周期：组件从被创建到挂载到页面中运行，再到组件不用时卸载的过程

意义：组件的生命周期有助于理解组件的运行方式、完成更复杂的组件功能、分析组件错误原因

在生命周期的不同阶段，会**自动调用执行的函数**，为开发人员在不同阶段操作组件提供了时机。

| 钩子 函数            | 触发时机                  | 作用                               |
| -------------------- | ------------------------- | ---------------------------------- |
| constructor          | 创建组件时，最先执行      | 1. 初始化state  2. 创建Ref等       |
| render               | 每次组件渲染都会触发      | 渲染UI                             |
| componentDidMount    | 组件挂载（完成DOM渲染）后 | 1. 发送网络请求   2.DOM操作        |
| componentDidUpdate   | 组件更新（完成DOM渲染）后 | DOM操作，可以获取到更新后的DOM内容 |
| componentWillUnmount | 组件卸载（从页面中消失）  | 执行清理工作（比如：清理定时器等） |

### 1. 挂载阶段

执行时机：组件创建时（页面加载时）

执行顺序：constructor() -> render() -> componentDidMount()

### 2. 更新阶段

更新阶段会执行两个钩子： render() -> componentDidUpdate()

三种方法可以触发组件更新

- 调用 setState 。它能改数据并且更新页面
- 调用forceUpdate() 
- 组件接收到新的 props

### 3. 卸载阶段

执行时机：组件销毁

定时器和一些事件不会随着组件的销毁而被移除, 故在卸载阶段可以执行清理工作



## 15. 组件性能优化

在完成功能的前提下做性能优化,有两点

- 减轻 state: 只存储跟组件渲染相关的数据(比如：count / 列表数据 / loading 等), 不做渲染的数据不放在 state 中, 比如定时器 id 等
- 避免不必要的重新渲染: 由于组件的更新机制导致父组件更新子组件也跟着更新, 当子组件数据没有变化时也会跟随父组件重新渲染, 可以通过钩子函数`shouldComponentUpdate(nextProps, nextState)` 来控制该组件是否重新渲染, 其是一个更新阶段的钩子函数, 在组件重新渲染前执行

```jsx
class Hello extends Component {
// 执行顺序是 shouldComponentUpdate() => render()
    shouldComponentUpdate() {
        // 根据条件，决定是否重新渲染组件 true 表示重新渲染
        return false
    }
    render() {…}
}
```

- 纯组件: 即 `React.PureComponent` , 与 `React.Component  `功能相似,其内部自动实现了 `shouldComponentUpdate` 钩子函数,无需手动比较

```jsx
// 其内部通过分别 对比 前后两次 props 和 state 的值，来决定是否重新渲染组件
// 在性能优化的时候可能会用到纯组件，请勿所有组件都使用纯组件，因为纯组件需要消耗性能进行对比
class Hello extends React.PureComponent {
    render() {
        return (
        	<div>纯组件</div>
        )
    }
}
```



## 16. Hooks

### 1. 概念 

> **Hooks 只能在函数组件中使用**

Hooks 是 React 16.8 的新增特性, 是一些可以让你在函数式组件里使用 React state 及生命周期等特性的函数. 为 **函数组件** 提供状态、生命周期等原本 在 Class 组件中才提供的功能.

### 2. 优势

类组件之间很少继承,也很少相互访问, **根据状态来渲染UI这件事** 上发挥的并不好

而函数式组件本身简单,更好胜任这件事,通过 hooks 让组件有了维护状态的能力,也提升了组件的逻辑复用能力

### 3. 使用策略

react 没有计划移除 class , 故 hooks 和现有代码可以同时工作, 推荐渐进式的使用,如 新功能用 hooks , 复杂功能实现不了的继续用 class 

### 4. 核心 API

#### 1.   useState 

​	在函数式组件中使用状态时使用, 其可以为函数式组件提供状态

```jsx
// 1. 导入调用函数, useState 是 hook，hook是 use 开头的函数
import { useState } from 'react'
const Count = () => {  
  // 返回值是一个数组  0 是状态初始值, 可以是任意值,这点跟 class 组件有所不同
  // class 组件中的 state 必须是一个对象
  const stateArray = useState(0)
  // 状态值 => 0 
  const state = stateArray[0]
  // 修改状态值的函数 => 1
  const setState = stateArray[1]
  // 可以使用数组解构简化代码,这也是开发中最常用的
  // 解构名称可以自定义,但一般须带有意义, 且修改的函数一般以 set 开头后面跟上状态名称
  const [state, setState] = useState(0)
  return (
    <div>
      {/* 展示状态值 */}
      <h1>useState Hook -> {state}</h1>
      {/* 点击按钮，让状态值 +1 */}
      <button onClick={() => setState(state + 1)}>+1</button>
    </div>
  )
}
```

useState 有两种格式

- 直接传入初始值: 如  `useState(0)`   `useState('abc')`
- 传入回调函数: 如 `useState(() => { return 初始值 })` , 返回值就是当前状态的值  , 该回调函数只会执行一次,  当组件第一次渲染会执行初始状态, 当状态值发生改变时会导致页面第二次渲染, 此时组件中代码逻辑会再次执行, 即会再次调用 useState(0) , 但此时的结果会是最新的状态值而非初始值



#### **2.  useEffect**

> effect 只能是一个同步函数，不能使用 async

  事物的主要作用之外的，就是副作用. 在函数式组件中, 主作用是根据数据和状态渲染 UI, 除此之外的操作都可以称之为副作用, 主要包括数据请求、手动修改 DOM、开启/清空定时器，添加事件监听，删除事件, localStorage 操作等

```jsx
// 1. 导入 useEffect 函数
import { useEffect } from 'react'
// 2. 使用 useEffect ,传入回调函数, 可以多次定义并顺序执行
useEffect(() => {
	console.log('useEffect 1 执行了,可以做副作用')
})

// 当只传入回调函数时, 每次页面重新渲染所有副作用函数都会再次执行,这显然是没必要的
useEffect(() => {
//  不带第二个参数,会导致每次页面更新都会执行 
	console.log('useEffect 2 执行了,可以做副作用')
})
// 解决: 第二个参数传入执行副作用函数的依赖项, 它决定了什么时机执行回调函数
useEffect(() => {
// 1. 带第二个参数,参数是空数组,这样只会在组件渲染时执行一次,一般发请求或事件绑定时会使用
	console.log('useEffect 1 执行了,可以做副作用')
}, []) // 这种情况只会执行一次
useEffect(() => {
// 2. 带第二个参数(数组格式),并指定依赖项,会初始执行一次,并在依赖项的值发生变化时继续执行
	console.log('useEffect 1 执行了,可以做副作用')
},[count, list]) // 初始执行一次, 等 count 和 list 值发生变化才会再执行
// 回调函数还能返回一个清理函数,用来清理副作用
useEffect(() => {
	console.log('useEffect 1 执行了,可以做副作用')
    // 清理函数会在组件卸载时以及下一次副作用函数调用之前执行
    // 类似于 class 中的 componentWillUnmount
    return （）=>{ /* 做清理工作*/ } // 清理函数
}, [])
```

**将组件状态逻辑提取到可重用的函数（自定义 Hooks）中，实现状态逻辑复用。**

除了使用内置的 Hooks ，还能创建自己的 Hooks(自定义 Hooks)来实现不同状态的**逻辑复用**

- 自定义 Hooks 是一个函数，**约定函数名称必须以 use 开头，React 就是通过函数名称是否以 use 开头来判断是不是 Hooks**
- Hooks 只能在函数组件中或其他自定义 Hooks 中使用，否则，会报错！
- 自定义 Hooks 用来提取组件的状态逻辑，根据不同功能可以有不同的参数和返回值（就像使用普通函数一样）



#### 3. useRef

在 React 中进行 DOM 操作时，用来获取 DOM, 其作用是 **返回一个带有 current 属性的可变对象，通过该对象就可以进行 DOM 操作了。**

```jsx
// 1. 导入 useRef
import React, { useRef } from 'react'
// 获取 dom 时一般都设置为 null , 返回值是一个包含 current 属性的对象
// 只要在 React 中进行 DOM 操作，都能通过 useRef 来获取 DOM（比如，获取 DOM 的宽高等）
// useRef 不仅仅可以用于获取当前组件 DOM ，还可以获取子组件 dom
// ref 不能使用在函数式组件上，因为函数式组件没有实例！要配合 forwardRef 使用!
 const refTxt = useRef(null) // 获取 dom
 const refCom1 = useRef(null) // 获取组件 dom
 console.log(refTxt) // { current: null } 初次渲染是 null
 const click = () => {
    console.log(refTxt, refCom1) // { current: input } { current: Com1 } 
    console.log(refCom1.current.state.a) // 100
    console.log(refTxt.current.value) // ' '
    refTxt.current.style.backgroundColor = 'red'
  }
  return (
    <div>
      useRef, <input ref={refTxt} />{' '}
      <button onClick={click}>点击，获取input中的值</button>
      <br />
      <Com1 ref={refCom1} />
    </div>
  )
```

函数式组件有一个缺点,就是无法在多次渲染之间共享数据, 这个问题可以通过  useRef 来解决

```jsx
 const [count, setCount] = useState(0)
  let timeId = null
  console.log(timeId) // null 
  const timeRef = useRef(null)
  // timeRef.current = 你需要在多次渲染之间共享的数据
  // 这个方法返回的 ref 对象在组件整个生命周期内不变
  useEffect(() => {
    timeRef.current = setInterval(() => {
    // 当执行 setCount 时组件会重新渲染, 而当重新渲染时会重复执行 let timeId = null
    // 这就导致 timeId 被重置了 无法保存也无法清除, 通过调用 useRef 解决这个问题
      setCount((count) => count + 1)
    }, 1000)
    console.log(timeRef.current)
  }, [])
  const hClick = () => {
    console.log(timeRef.current) // 关闭根组件严格模式才会生效
    clearInterval(timeRef.current) // 直接清除无效 通过 useRef 清理
  }
  return (
    <div>
      count:{count}
      <button onClick={hClick}>点击停止定时器</button>
    </div>
  )
}
```



#### 4. useContext

useContext 可以帮助我们跨越组件层级直接传递变量，实现数据共享。

其作用就是对组件所包含的所有后代组件提供全局的共享数据

```jsx
// 使用步骤分为三步: 1. 导入并调用该方法,然后在当前组件导出
import { createContext } from 'react'
export const { Provider, Context } = createContext()
// 2. 在当前组件使用 Provider 包裹根组件,并通过 value 属性提供要共享的数据
return (
  <Provider value={ 这里放要传递的数据 }>
  	<根组件的内容/>
  </Provider>
)
// 3. 在任意后代组件中，导入 useContext 和 Context ,调用并接收数据即可使用
import React, { useContext } from 'react'
import { Context } from './index'
const 函数组件 = () => {
    const 公共数据 = useContext(Context)
    return ( 函数组件的内容 )
}
```



## 17. redux

> Redux 并不只为 react 应用提供状态管理， 它还支持其它的框架。

#### 1. 概念

Redux 是 JavaScript 应用的状态容器，提供可预测的状态管理。类似于 Vue 中的 Vuex.

其作用是 **集中式存储和管理应用的状态**, 能简化组件之间通讯问题,数据流清晰

分为**三个核心概念**

#### 2. action

action 是一个 js 对象, 包含了两个属性, 分别是 

- type: 标识属性, 值是字符串. 用来区分操作的动作
- payload: 数据属性,是一个可选属性, 表示本次动作携带的数据

其只是描述了**有事情发生**, 无法更新 state 数据, 更新数据交给 reducer 来处理

```jsx
// 实际开发中常根据动作的不同设置不同的单独 action 文件
{ type： 'decrement', count： 2 }
// 类型文件一般也是单独设置文件夹然后导入使用, 更方便开发与维护
// 单独文件 constant.js 中导出类型
export const ADD = 'todos/add'
// action 中导入类型
import { ADD } from '../constant'
export const addListAction = (payload) => ({type: ADD, payload})
```

#### 3. reducer

reducer 是一个**纯函数** , 只负责**初始化状态**和**修改状态**,  即根据传入的 action，返回新状态

```jsx
const initState = [ {id: 1, name: '吃饭', isDone: false} ]
// 一般而言会定义初始状态, 即第一次返回的值
// state 代表上一次的状态数据, action 代表所需要执行的动作和数据,可以解构
const reducer = (state = initState, {type, payload}) => {
  switch (type) {
    case 'add':
      // 返回新的state
      return state + 1
    case 'addN':
      // 返回新的state
      return state + payload
    default:
      return state
  }
}
// 实际开发中也会根据实际情况设置不同的 reducer 文件, 通过目录下的 index 整合到一起
import { combineReducers } from 'redux'
// 导入不同的处理 reducer 文件,通过 combineReducers 方法整合到一起
import todos from './todos'
import filter from './filter'
export default combineReducers({
  todos,
  filter
})
```



#### 4. store

是 redux 的核心, 主要是整合 action 和 reducer. 有以下特点

- 一个项目**只有一个 store**
- 维护应用的状态，通过 `store.getState()` 获取初始状态
- 创建 store 时**接收 reducer 作为参数**：`const store = createStore(reducer)`
- 发起状态更新时，需要分发 action：`store.dispatch(action)`
- 状态更新后会触发 `store.subscribe(() => {}) `  函数, 通过该函数通知组件更新数据

```jsx
// store.subscribe 实际是添加了订阅者, 当状态更新后 store 会通知订阅者
// 在当前组件执行  store.subscribe 方法来更新页面
const [val, setVal] = useState({})
const unSubscribe = store.subscribe(() => {
  // 状态改变时，执行相应操作
  console.log('数据变化了....')
  // 定义空对象来更新页面
  setVal({})
})
// 可以通过 unSubscribe 取消订阅 取消监听状态变化
unSubscribe()
// 每个组件都这样设置太过繁琐, 可以在根组件设置,这样只要状态变更所有页面都会重新渲染
// 在 src/index.js 中设置, 由于 react 有 diff 算法所以对整体影响并不大
store.subscribe(() => {
   ReactDOM.createRoot(document.getElementById('root')).render( <App />)
})
```

#### 5. Redux 代码执行过程

##### 1. 获取默认值

只要创建 store，Redux 就会调用一次 reducer， 且 type 是一个随机值, 确保第一次调用返回状态默认值, 即使用 `store.getState()` 方法所获取到的值

##### 2. 更新状态

在组件内需要更新状态时, 先分发动作  `store.dispatch(action)`  ,  redux 内部就会调用 reducer , 传入上一次的状态 state 和动作 action , 等待 reducer 执行完毕后返回最新状态, 最后 store 用最新状态替换旧状态, 此时状态更新完毕

##### 3. 通知订阅者

状态更新完毕后, store 会通知订阅者, 即执行  ` store.subscribe` 这个方法



## 18. React-redux

由于 redux 并不只为 react 应用提供状态管理， 它还支持其它的框架。所以在 react 中直接使用 redux 很不方便, 如

- 每个组件都需要单独导入 store 
- 通知订阅者写在根组件上写法不友好

故官方推出了 React-redux 库来解决这些问题, 完美融入 react, 有三个核心 API

#### 1. Provider

解决每个组件都需导入 store 的问题,  将 Provider 包裹在根组件上

```jsx
import { Provider} from 'react-redux'
createRoot(document.getElementById('root')).render(
  // 使用 Provider 包裹根组件,所有组件不用再导入 store 了
  <Provider store={ store }>
    <App />
  </Provider>
)
```

#### 2. useSelector

用来获取公共状态, 代替了 `store.getState()` 方法, 且 state 状态变化后会自动更新

```jsx
import {useSelector} from 'react-redux'
// const 状态 = useSelector(store的数据 => 你需要的部分)
// state 包含所有的 store 状态数据, 可以通过回调函数筛选需要的部分
const list = useSelector((state) => state.todos )
```

#### 3. useDispatch

用来派发 action 动作, 通知 store 更新状态. 需先调用方法得到返回值再来执行

```jsx
import { useDispatch } from 'react-redux'
// 先调用方法得到 dispatch 函数
const dispatch = useDispatch()
// 调用 dispatch 方法传入 action 动作 , 实际开发中 action 会单独定义好导入调用执行
import { sendAction } from './store/action'
dispatch(sendAction())
```



## 19. redux 中间件

默认情况下，Redux 自身只能处理同步数据流。但是在实际项目开发中，状态的更新、获取，通常是使用异步操作来实现。而 Redux 无法直接实现,就需要通过 Redux 中间件机制来实现

#### 概念

中间件，可以理解为处理一个功能的中间环节 , 用来在不损害原功能的前提下，引入额外的代码来拓展功能

- 优势：可以串联、组合，在一个项目中使用多个中间件

**Redux 中间件用来处理状态更新，也就是在状态更新的过程中，执行一系列的相应操作**

#### 触发时机

无中间件时:  **`dispatch(action) => reducer`**

Redux 中间件执行时机：**dispatch(action) => 执行中间件代码 => reducer**

引入中间件后 触发 dispatch 方法执行的是中间件封装处理后的 dispatch , 其最终一定会调用Redux 库自己提供的 dispatch 方法

有以下常见中间件

####  1. redux-thunk

​	该中间件允许 redux 处理函数形式的 action , 这样在函数内部就可以执行异步操作,通常用来发	异步请求

```jsx
// 之前异步操作都写在组件中, 请求到数据再创建 action,这使得维护非常不便
// 1. 先安装注册  yarn add redux-thunk 在 store/index.js 中注册
import { createStore, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'
// redux 提供了 applyMiddleware 方法来注册中间件
export default createStore(rootReducer, applyMiddleware(thunk))

// 返回一个函数, 形参就是 dispatch 
const addTodo = (name)=> {
  return async (dispatch) =>{
    const res = await 异步动作()
    dispatch({type: 'todos/add', payload: name})
  }
}
// 组件中调用传参
dispatch(addTodo('学习redux'))
```



#### 2. redux-logger 

该中间件可以追踪 redux 的操作日志, 在控制台打印出日志信息

```jsx
// 安装 yarn add redux-logger 导入 store/index.js 中注册
import { createStore, applyMiddleware } from 'redux'
import logger from 'redux-logger'
import thunk from 'redux-thunk'
import rootReducer from './reducers'
// 多个中间件可以用逗号隔开, 但是 logger 必须设置成最后一中间件
const store = createStore(rootReducer, applyMiddleware(thunk, logger))
```



#### 3. redux-devtools-extension

引入该中间件, 能通过 chrome 开发者工具调试跟踪 redux 状态.

> 需先安装 react 和 redux 的开发者工具

```jsx
// 需先安装 react  redux  开发者工具 即 chrome 浏览器插件
// 注册方式跟其他中间件有些不一样,须通过 composeWithDevTools 包裹所有中间件
import { createStore, applyMiddleware } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'
const store = createStore(reducer, composeWithDevTools(applyMiddleware(中间件..)))

export default store
```



## 20. React 路由

众所周知，路由是一套映射关系。在 React 中路由是指路径和组件的对应关系

简单来说就是配置路径和组件（配对）

#### 模拟 hash 路由

```jsx
// 核心思路就是维护 hash 的状态， 当状态变化时修改它，根据 hash 的值来决定显示哪个组件
import React, { useEffect, useState } from 'react'
import Home from './pages/Home.jsx'
import Search from './pages/Search.jsx'
import Comment from './pages/Comment.jsx'

export default function App () {
	// 记录 hash 的状态
  const [curHash, setCurHash] = useState('')

  useEffect(() => {
    const onChange = () => {
      // console.log(window.location.hash)
      setCurHash(window.location.hash.slice(1))
    }
    onChange()
      // 页面加载 注册 hashchange 事件，当 hash 值发生变化时触发
    window.addEventListener('hashchange', onChange)
    return () => {
       // 清除注册的事件
      window.removeEventListener('hashchange', onChange)
    }
  }, [])
  return (
    <div>
      <ul>
        <li><a href="#/home">首页</a> </li>
        <li> <a href="#/comment">评论</a> </li>
        <li> <a href="#/search">搜索</a> </li>
      </ul>
      <hr />
      {curHash === '/home' && <Home />}
      {curHash === '/search' && <Search />}
      {curHash === '/comment' && <Comment />}
    </div>
  )
}

```

#### 基本使用

react 中使用需先下包 `react-router-dom` , 该包有 5 和 6 两个大的版本，改动较大

#### 5.X版本

> 2021年8月出的5.3版本

该版本有三个核心组件 `Router` ， `Route`，`Link`

- **Router**： 路由模式，用来包裹根组件。具体路由模式由引入的 router 决定

- **HashRouter**： 使用 URL 的哈希值实现，原理是监听 window 的`hashchange` 事件
- **BrowserRouter**：使用 H5 的 history.pushState() API 实现，监听 `popstate` 事件
- **Link**： 用于**指定路由导航**。最终渲染成 a 标签，to 属性渲染成 href 属性
- **NavLink**：一个特殊的 `Link` 组件，可以用于指定当前导航高亮
- **Route**：匹配路由规则，浏览器路径和 path 匹配成功就会显示该组件。path 代表浏览器路径，component 代表对应的组件，匹配成功不会停止，可能会匹配多个组件
- **Switch**：使用该组件可确保不管有多少路由规则匹配成功都只会渲染第一个匹配上的组件
- **Redirect**：路由路径重定向跳转。from 属性表示当前路径， to 表示要重定向的路径

```js
// HashRouter: hash 模式	BrowserRouter： history 模式
// 二者都需要包裹整个根组件，且一个项目中只能有一个 Router，一般都会重命名为 router
import { BrowserRouter as Router } from 'react-router-dom'
import { HashRouter as Router，Link，Route，NavLink，Redirect} from 'react-router-dom'
export default function App () {
  return (
   // 包裹根组件来接管项目路径
    <Router>
      <h1>react路由基本使用</h1>
        <Link to="/comment">评论</Link>
        <Link to="/search">搜索</Link>
 // activeClassName 用于指定高亮的类名，默认 active 。一般不去修改。
 // NavLink 还有 exact 属性，表示精确匹配
		<NavLink to="/xxx" activeClassName="active">链接</NavLink>
		<NavLink to="/search" exact>搜索</NavLink>
 //  Route 也有 exact 属性，表示精确匹配，默认是模糊匹配
        <Route path="/comment" component={Comment} />
        <Route path="/search" component={Search} />
            
        <Switch>
           // 一般路径匹配 / ，都需要配置 exact 属性 
          <Route path="/" exact component={Home} />
          <Route path="/article" component={Article} />
          <Route path="/article/123" component={ArticleDetail} />
          // 重定向路由路径 ，一般加上 exact 属性
          <Redirect from="/" to="/comment" exact />
          // 匹配 404 页面，不设置 path 属性，将对应路由放在 Switch 组件的最后位置
          <Route component={Page404} />
        </Switch>
    </Router>
  )
}
```

##### 编程式导航

React 中页面跳转有两种方式，一种是通过 **` Link`** 标签点击跳转，一种是通过**编程式导航**跳转

react-router 中提供了 `useHistory` 这个  API 来实现编程式跳转

```jsx
import { useHistory } from 'react-router'

export default function App() {
	// 导入 useHistory 方法并调用，得到编程式跳转方法
  const history = useHistory()
  // history 中有 push 方法来指定要跳转到哪个页面
  history.push('/find'）
  
  // history 中有 go 方法来前进或后退到某个页面
  // 参数 n 表示前进或后退页面数量（比如：-1 表示后退到上一页）
  history.go(-1) 
  
  // history 中有 replace 方法来替换当前页面，并替换记录
  // 和 push 方法区别在于 push 方法会向历史记录中添加一条记录，而该方法会替换记录
  history.replace('/frend') 
}
```

##### 路由传参

- params 传参：在 Route 上的 path 里通过 `/:id/:name` 的形式传参

```jsx
// 例如 <Route path="/article/:age/:name" component={Article} />
// 在路径上会显示为 /article/18/jack  有两种方法接收传递的参数
// 1. 通过 props.match.params 接收
function Article(props){
    console.log(props.match.params)  // { age: 18, name: jack }
}
// 2. 通过 hooks 提供的 useParams 方法
import { useParams } from 'react-router-dom'
const params = useParams()
console.log(params) // { age: 18, name: jack }
```

- search 传参： 在 Link 上通过 ?分隔，&符号分连传参

```jsx
// Route 上无需特殊处理，Link 上用 ?分隔&相连
<Link to="/layout/user?name=jack&age=18">用户</Link>
<Route path="/layout/user" component={Layout} />
// 组件内用 props.location.search 接收，接受的参数需要单独处理编码
// 可以借助 query-string 包的 stringfy() 和 parse() 方法转换
// 还可以借助 querystring 包的 stringfy() 和 parse() 方法转换
// 构造函数 new URLSearchParams() 也可以，不过比较麻烦
import qs from 'query-string'
import querystring from 'querystring'
const params = new URLSearchParams(props.location.search.slice(1))
const {name,age } = qs.parse(props.location.search.slice(1))

```

- state 传参： 在 Link 上通过 state= 参数对象来传参

```jsx
// Link 上传入一个 state 对象 组件内通过 props.location.state 来接收传递的对象
<Link to={{pathname:'/layout/user',state:{name:'jack',age:18}}}>首页</Link>
<Route path="/layout/user" component={Layout} />
// state 可以任意起名，但是一般不变更名字。 刷新页面 state 不会丢失
function Layout(props) {
  console.log(props.location.state) //  {name: 'jack', age: 18}
}
```

##### 路由嵌套

路由嵌套可以任意搭配，但是路径一定要一级一级匹配上，先匹配父级路由再匹配子路由

```jsx
// 通过 /home 可以匹配Home父组件  再通过/list匹配子组件
<Route path="/home/list" component={List} />
```



#### 6.X 版本

> 2022年3月出的 6.3 版本

将 `Switch` 升级为 `Routes`, 当路由嵌套时，子路由再无需补充完整路径而可以使用简短语法

`Route ` 中的子组件由 `component` 升级成 `element` , 且需使用标签写法

使用 `Route` 中的 `path` 路径 *  取代了 `<Route exact>`

```js
import { BrowserRouter, Routes,Route, Link} from "react-router-dom";
 // 父级路由
function App() {
  return (
    <BrowserRouter>
      // Routes 代替了 Switch
      <Routes>
        // element 代替了 component
        <Route path="/" element={<Home />} />
        <Route path="users/*" element={<Users />} />
      </Routes>
    </BrowserRouter>
  );
}
// 子路由
function Users() {
  return (
    <div>
      <nav>
        <Link to="me">My Profile</Link>
     //	<Link to={{ pathname: "/home", state: state }} />
      // v6 中 升级了 Link 组件,让其可以单独使用 state
      	<Link to="/home" state={state} />
      </nav>
      <Routes>
         // 不用再写 /users/me /users/:id 而可以直接写子路由这一级
        <Route path=":id" element={<UserProfile />} />
        <Route path="me" element={<OwnUserProfile />} />
      </Routes>
    </div>
  );
}
```

##### 编程式导航

用 `useNavigate` 代替了 `useHistory` , 有两个参数, ` navigate(to, { replace: true })` 用来替换当前页,  `navigate(to, { state })` 用来接收参数

```js
// React Router v6 引入了一个新的导航 API useNavigate , 代替了 v5 的 useHistory
// This is a React Router v5 app
import { useHistory } from "react-router-dom"

function App() {
  let history = useHistory();
  function handleClick() {
    history.push("/home");
  }
  return (
    <div>
      <button onClick={handleClick}>go home</button>
    </div>
  );
}

// This is a React Router v6 app
import { useNavigate } from "react-router-dom";

function App() {
  let navigate = useNavigate();
  function handleClick() {
    navigate("/home");
  }
  return (
    <div>
      <button onClick={handleClick}>go home</button>
    </div>
  );
}
```

##### 重定向

使用 `Navigate` 代替了 `Redirect` 

```js
import { Navigate } from "react-router-dom";

function App() {
  return <Navigate to="/home" replace state={state} />;
}

// Change this:
<Redirect to="about" />
<Redirect to="home" push />

// to this:
<Navigate to="about" replace />
<Navigate to="home" />

```

##### 页面跳转

简化了` go, goBack, goForward` 等方法

```js
// This is a React Router v5 app
import { useHistory } from "react-router-dom";

function App() {
  const { go, goBack, goForward } = useHistory();

  return (
    <>
      <button onClick={() => go(-2)}>
        Go 2 pages back
      </button>
      <button onClick={goBack}>Go back</button>
      <button onClick={goForward}>Go forward</button>
      <button onClick={() => go(2)}>
        Go 2 pages forward
      </button>
    </>
  );
}

// This is a React Router v6 app
import { useNavigate } from "react-router-dom";

function App() {
  const navigate = useNavigate();

  return (
    <>
      <button onClick={() => navigate(-2)}>
        Go 2 pages back
      </button>
      <button onClick={() => navigate(-1)}>Go back</button>
      <button onClick={() => navigate(1)}>
        Go forward
      </button>
      <button onClick={() => navigate(2)}>
        Go 2 pages forward
      </button>
    </>
  );
}
```



