---
title: Vue2 总结
tags: Vue2
abbrlink: 8c5fb31e
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/vue2.png
---



# Vue2 总结

### 1. webpack

1. webpack 是一个 node 第三方模块打包工具, 可以识别代码，翻译，压缩，整合打包，能减少文件数量，压缩代码体积，提高网页打开速度。

2. 使用前需先初始化包环境，下载 `webpack` `webpack-cli` 等模块，最后在 package.json 中自定义命令

3. 使用需先配置入口文件，且将需要引用的文件导入到入口文件中才会参与打包，然后执行自定义的命令，最终生成打包后的 dist 文件夹中的 main.js

4. ###### webpack 配置修改： 分为入口，出口，打包模式，loader 加载器，plugins 插件配置

   - 入口： 通过 `entry` 设置入口文件路径，可以是相对路径

   ```
   entry: './src/main.js', // 入口, 可以是相对路径
   ```

   - 出口： 通过`output` 设置出口路径， 必须是绝对路径，还可以自定义出口文件名

   ```js
   output: { // 出口, 必须是绝对路径
   		path: path.join(__dirname, 'dist'),
   		filename: 'bundle.js',
   	},
   ```

   - 打包模式： 分为`production` 生产模式和 `development` 开发模式

   ```js
   mode: 'production',	 // production: 生产模式 development: 开发模式	
   ```

   - 服务器配置：通过 `devServer` 设置端口号，打开方式， host 地址等

   ```js
   devServer: {
   		port: 3000, // 端口号
   		open: true, // 自动打开浏览器
   	},
   ```

   - loader 加载器：webpack 默认只打包 js 文件，如需打包其他文件类型就需要配置对应的 loader 规则，通过配置 `loader` 让 webpack 有了打包其他文件类型的能力

   ```js
   module: {	// 配置 loader
   	rules: [	// loader 规则 一个对象表示一个匹配规则
   		{
   			test: /\.css$/i,
   			use: ['style-loader', 'css-loader'],
   		}，
   	]
   }
   ```

   - plugins 插件：通过配置 `plugins`  能给 webpack 带来更多功能

   ```js
   plugins: [ //插件配置是一个数组，一个对象表示一个插件 通过 new 关键字生成实例对象
   	new HtmlWebpackPlugin(),
       new MiniCssExtractPlugin()
   ]
   ```

   - Source Map： 是一个信息文件，里面存储着代码的位置信息，开发环境下默认启用了该功能，当报错时可以提示错误的位置信息，但是记录的是压缩后的代码位置，会导致报错的行数与实际行数不一致，可通过配置`devtool` 保证行数一致，不受压缩代码的影响

   ```js
    devtool: 'nosources-source-map', 
   ```

   - resolve： 用于配置模块如何被解析，常见的如 `alias`，`modules`,`extensions`等

   ```
   // modules 默认值是 ['node_modules'],指默认先去当前目录的 node_modules 里找模块，没找到就往上一级的 node_modules 找，一直没找到就报错
   	resolve: {
   		modules: [path.resolve(__dirname,'node_modules')],
   		alias: {
        	 '@': path.join(__dirname, './src/')
      		 }
   	}
   // extensions 默认值是[".wasm",".mjs",".js",".json"],如果导入的模块没有扩展名，webpack 会根据设置的扩展名一 一查找，谁在前就先查找谁，所以导入 js 文件可以省略 .js
   // alias 是给文件路径取个别名，这样每次导入模块时就不用写一长串了，如 @  表示 src
   ```

### 2.脚手架

	1. 脚手架是 Vue 官方提供的一套标准的文件夹+文件结构+ webpack 配置，快速搭建项目基本		环境，使用脚手架可以做到 0 配置，开箱即用，快速搭建项目基本开发环境
	2. 使用脚手架需安装全局包，得到终端命令，然后创建脚手架项目

```js
// 1. 下包	yarn global add @vue/cli
// 2. 使用终端命令创建脚手架项目	vue create 项目名（名称不能带大写字母中文和特殊符号）
```

​	3.  main.js 是项目打包入口，App.vue 是 vue 页面入口，index.html 是浏览器运行的文件，三者关系是 App.vue => main.js => index.html

	4. 配置文件由原来的 webpack 升级成了 vue.config.js

### 3. Vue 概念及指令修饰符

1. ##### Vue是一个JavaScript渐进式框架，渐进式就是按需逐渐集成功能，库是方法的集合，而框架是一套拥有自己规则的语法。学习 Vue 可以使开发更快速，更高效，且市场占有率高。

2. ##### vue 指令，官方封装好的指令

| 指令名  | 作用                                                     | 说明                                                         |
| ------- | -------------------------------------------------------- | ------------------------------------------------------------ |
| v-bind  | 给标签动态绑定属性值                                     | 可以简写成 :属性名=‘属性值’                                  |
| v-on    | 给标签绑定事件，还带有修饰符，包括事件修饰符和按键修饰符 | 简写 @事件名=‘事件处理函数’，不带参数默认第一个参数是事件对象，带参数用 $event 代替事件对象 |
| v-if    | 使标签显示或隐藏，当值为 true时显示                      | 本质是添加或者移除 dom 元素                                  |
| v-show  | 使标签显示或隐藏，当值为 true时显示                      | 本质是修改 dom 元素的 display 为 none                        |
| v-for   | 在标签上使用循环创建标签                                 | 必须带 key 值，key 值不能重复                                |
| v-html  | 设置标签显示的内容                                       | 可以解析标签名                                               |
| v-text  | 设置标签显示的内容                                       | 不能解析标签名                                               |
| v-model | 在表单标签上可以双向绑定标签的 value                     | 在 select 上绑定的是 option 的 value 值，多选框绑定的是 checked 值 |

3. ##### 修饰符：事件修饰符

| 修饰符名       | 作用                          |
| -------------- | ----------------------------- |
| .stop          | 阻止事件冒泡                  |
| .prevent       | 阻止默认事件                  |
| .lazy          | 让事件只触发一次              |
| @keyup.enter   | 监测回车按键                  |
| @keyup.esc     | 监测 esc 返回按键             |
| v-model.number | 以 parseFloat 转成数字类型    |
| v-model.trim   | 去除首尾空白字符              |
| v-model.lazy   | 在 change 时触发而非 inupt 时 |
| .once          | 只执行一次这个事件            |

4. ##### 插值表达式：在标签里面可以使用双大括号包裹表达式，转换成表达式结果展示

5. ##### MVVC ：是种软件架构模式，通过数据双向绑定让数据自动的双向同步，减少操作 dom ，提高开发效率

   - M：   model数据模型          (data里定义)	
   - V：    view视图                   （页面标签）
   - VM： ViewModel视图模型  (vue.js源码)

6. ##### MVC: 也是一种设计模式, 组织代码的结构, 是model数据模型, view视图, Controller控制器, 在控制器这层里编写js代码, 来控制数据和视图关联

### 4. v-for 

v-for 循环中，有一个监测机制，当修改循环的数组时

- 直接修改原数组会触发机制，更新页面
- 没有修改原数组，通过赋值给原数组也会触发
- 通过下标修改数组数据，或者通过数组的一些方法如`slice` 、`filter` 、`concat` 等直接修改也不会触发机制，不会造成页面刷新
- 如果通过以上方法修改数组不造成页面更新，可以通过 `this.$set()` 方法来触发页面更新

```js
// this.$set(a,b,c);	a 表示要修改的对象或数组	b 表示要修改的属性，对象用字符串的键表示，数组用下标数字表示		c 是要修改的值 
// 通过这个方法可以修改 v-for 监测不到的数据修改，更新页面
data() {
    return {
      arr: [5, 3, 9, 2, 1],
    };
  },
methods: {
      updateBtn() {
       //   this.arr[0] = 1000; 直接修改页面不会更新
        this.$set(this.arr, 0, 1000); // 页面更新
      }
 }     
```

当数据发生变化时， Vue 会尽可能的就地复用来更新数据

- 无 key 会尽可能就地复用
- 有 id 优先用 id，无 id 用索引
- key 值不能重复，且必须是数字或字符串

.vue文件中的template里写的标签, 都是模板, 都要被vue处理成虚拟DOM对象, 再生成真实DOM片段, 才会渲染显示到真实DOM页面上

- 虚拟 dom：本质就是一个JS对象, 保存DOM关键信息
- 可以提高DOM更新的性能, 不频繁操作真实DOM, 在内存中找到变化部分, 更新真实DOM(打补丁)

### 5. 动态 class 和动态 style

用 v-bind 给标签 class 、style 设置动态的值，当满足条件时该类名才会生效。

```js
<p :class="{类名: 布尔值}">动态class</p>
<p :style="{css 属性名: 属性值}">动态style</p>
```

### 6. 计算属性 computed

计算属性，就是定义一个数据，数据本身依赖另一些数据计算而得来的结果，计算的结果带有缓存特性，当依赖的数据没有发生变化时，会读取缓存的值，当依赖的数据发生变化才会触发重新计算

```js
// 如果只读取不赋值可以使用简单写法,此处的属性名可以直接在页面使用
computed : {
    属性名 (新值，旧值){
    return 值
 }
}
// 完整写法 又需要读取又要进行赋值操作
computed : {
   set(新值){
    // 赋值操作
},
   get(新值，旧值){
     return 值
 }
}
```

### 7.侦听器 watch

当 data/computed 属性值发生变化时可以触发对应函数，侦听数据的变化，只有当数据发生变化才会触发，不变化不触发

```js
// 简单侦听，只能侦听简单数据类型
watch: {
	"被侦听的属性名" (newVal, oldVal){
    }
}
// 深度侦听，可以侦听对象和数组等复杂数据类型
watch: {
    "要侦听的属性名": {
        deep: true, // 深度侦听复杂类型内变化
        immediate:true,	// 立即侦听，控制侦听器是否立即触发，默认值是 false
        handler (newVal, oldVal) {
            
        }
    }
}
```

### 8.组件

组件在 vue 中是指可复用的 vue 实例，封装了标签样式和 js ，当有重复的标签可复用时可以封装成组件，封装后组件之间作用域各自独立，互不影响，组件又分为全局组件和局部组件

- 全局组件： 在 main.js 中注册，注册后项目全局可用

```js
// 全局注册，注册后其他地方想使用直接用组件名即可
import 组件对象 from 'vue文件路径'
Vue.component("组件名", 组件对象)
// 补充一个全局注册组件的方法,此方法所有全局组件必须带 name , 组件名就是 name
// 在 components/index.js 中默认导出一个对象
// 原始写法
import PageTools from './PageTools/index.vue'
 export default {
   install(Vue) {
     Vue.component('PageTools', PageTools)
      ...
   }
 }
// webpack + proxy 打包写法,一次到位,无需重复注册组件
export default {
  install(Vue) {
      // 三个参数,分别是文件路径,是否深层次查找, 正则查找 .vue 文件
    const requireComponent = require.context('./', true, /\.vue$/)
    requireComponent.keys().forEach((item) => {
        // requireComponent.keys() 返回一个数组,包含目录下所有 .vue 文件的相对路径
        // requireComponent(item).default 返回对应组件对象,所以组件需配置 name 
      const moduleObj = requireComponent(item).default
      Vue.component(moduleObj.name, moduleObj)
    })
  }
}
```

- 局部组件： 在组件内部引入，注册其他组件，只在当前组件可用

```js
// 1. 创建组件
// 2. 引入组件	import Pannel from './components/Pannel_1'
// 3. 注册组件  components: {"组件名": 组件对象}
// 4. 在页面使用组件 <Pannel></Pannel>
```

默认情况下每个 vue 文件中的 style 标签会影响其他文件的样式，所以封装组件时给组件 style 标签添加上 scoped 标签，可以使样式只作用于当前组件，不造成样式污染

```js
<style scoped> // 原理是会自动给标签添加 data-v-hash 值属性, 所有选择器都带
```

### 9.组件通讯

每个组件的变量和值都是独立的，当组件之间相互传值时就需要用到组件通讯，父向子传值称之为单向数据流，不可直接修改

```js
// 父向子传值，通过自定义属性名传值，子组件通过 props 接收
<TodoMain @delFn="delFn" :list="showArr"></TodoMain>
props: ["list"], // 简单写法可以写成数组接收
props: {		// 完整写法,可以限制条件
    tittle: { 	// 接收的值
      type: String,	 // 值数据类型
      default: "购物车案例", // 不传的时候默认值
      required: true,	// 设置后为必传值，不传会报错
   } },
// 子向父传值	通过 this.$emit('自定义事件名',值) 触发父组件事件
this.$emit('delFn'，值）
metheds:{
    delFn(值）{}
  }
```

### 10.组件生命周期

从 vue 实例创建到销毁的过程称之为 vue 生命周期，有四个阶段八个钩子函数，分别是初始化/ 挂载/ 更新/ 销毁，钩子函数会随着生命周期阶段自动执行，在组件外部加了 `keep-alive` 后多了两个钩子函数

| **阶段** | **方法名**    | **方法名**  |
| -------- | ------------- | ----------- |
| 初始化   | beforeCreate  | created     |
| 挂载     | beforeMount   | mounted     |
| 更新     | beforeUpdate  | updated     |
| 销毁     | beforeDestroy | destroyed   |
| 缓存     | activated     | deactivated |

1. 初始化阶段：

   - beforeCreate：初始化事件和生命周期函数

   - created： 数据和方法在此处可以获取到，但是 dom 还未挂载获取不到，此处一般用来发送 ajax 请求

2. 挂载阶段：

   - beforeMount： 挂载前触发，此处 dom 还未真正挂载
   - mounted： dom 挂载后触发，此处可以获取真实 dom 和所有的数据方法，一般在此处获取真实 dom

3. 更新阶段：

   - beforeUpdate：数据发生改变时触发，此时数据已经更新但 dom 还未更新
   - updated：数据和 dom 都更新后触发

4. 销毁阶段

   - beforeDestroy：销毁实例前触发
   - destroyed：实例销毁时触发，一般在此处清除定时器、事件

5. 缓存阶段：加了缓存后组件不会销毁

   - activated：使用keep-alive的组件激活时触发
   - deactivated：使用keep-alive 的组件销毁时触发

### 11. axios

Ajax 是一种前端异步请求后端的技术,基于浏览器的 XMLHttpRequest 方法进行的封装,而 axios 是基于原生 ajax + Promise 技术封装通用于前后端的请求库，常用来在项目中处理请求

```js
Vue.prototype.$axios = axios	// 在 vue 中挂载到实例上
axios.defaults.baseURL= 地址	   // 添加基地址
```

### 12.自定义指令

当 vue 内置指令满足不了需求时，或者需对普通 dom 元素进行底层操作时可以使用自定义指令，获取 dom 标签，扩展额外功能

- 全局注册：在 main.js 中进行注册, 项目中任意组件使用只需要 v- 自定义指令名

```js
// 全局指令 - 到处"直接"使用
Vue.directive("自定义指令名", {
  inserted(el) {
    el.focus() // 触发标签的事件方法
  }
})
```

- 局部注册：只在当前组件中使用

```js
// inserted方法 - 指令所在标签, 被插入到网页上触发(一次)
// update方法 - 指令对应数据/标签更新时, 此方法执行
 directives: {
        focus: {
            inserted(el){	// 此处的 el 代表自定义指令所对应的当前组件对象
                el.focus()
            }
        }
    }
```

### 13. 进阶 Api

##### 1.  $nextTick()

- 在vue中 , DOM 更新是异步的,在更新数据后想获取到最新的 DOM 此时就可以用到this.$nextTick ,此外在 updated 钩子函数中也能获取到更新后的 DOM

```js
// this.$nextTick() 会返回 promise 对象，可以获取到更新数据后的 dom
 this.$nextTick(() => {
       console.log(this.$refs.myP.innerHTML); // 1
   })
```

##### 2. $refs

- 使用组件时，有时需要调用组件的方法，这时就可以用到 $refs 获取到组件对象

```js
<h1 id="h" ref="myH">我是一个h1</h1>
this.$refs.myH		// h1 对象
document.getElementById("h") // h1 对象
this.$refs.myH.fn() // 调用组件对象的方法，也可以访问组件的数据
```

##### 3. v-model

v-model 本质就是一个语法糖，向标签的 value 属性赋值，并绑定 input 事件，将收到的最新值赋值给变量。

```js
// 本质就是语法糖，绑定变量，绑定 input 事件
<AddBtn v-model="count"></AddBtn>
<AddBtn :value="count" @input="val => count = val"></AddBtn>
this.$emit('input', this.value + 1)
```

##### 4. 动态组件

多个组件使用同一挂载点并动态切换不同的组件，称之为动态组件，通过对组件变量名进行不同赋值达到动态更换组件的目的

```js
// 使用动态组件 component 标签，给 is 属性绑定组件变量名，切换变量名值显示不同组件
<component :is="comName"></component>
// data 中定义组件变量名，引入组件注册，切换变量名值为要显示的组件名
```

##### 5. 组件缓存

组件切换会导致组件被频繁销毁和重新创建, 性能不高。使用 Vue 内置的 keep-alive 组件, 可以让包裹的组件保存在内存中不被销毁，而是缓存起来。

```js
// keep-alive 可以提高组件性能，包裹的组件不会被销毁和重新创建，还会多了两个钩子函数
// activated 组件激活时触发 deactivated 组件失活时触发
// keep-alive 包含两个属性，exclude 不缓存值对应组件， include 只缓存值对应组件
// 如需使用 exclude/include 组件内需配置 name 值，组件名对应 name 值
<keep-alive exclude="组件名" include='组件名'>
        <component :is="comName"></component>
</keep-alive>
```

##### 6. .sync 修饰符

是对父子组件传值的一个语法糖,类似于 v-model ,方便父子之间传值与修改

```js
:visible.sync="dialogVisible" 	// element dialog 中就有用到
// 父组件给子组件传值,子组件通过 props 接收值, 并通过 emit 绑定修饰符事件,可以直接修改传过来的值
props:['visible'],
this.$emit('update:visible', 新值)
```

### 14. 插槽

当组件内某一部分的标签和内容不确定或者需要自定义时可以使用到插槽技术，在组件内用 `slot` 占位，使用组件时传入具体的标签，会替换掉 `slot` 的默认内容。插槽又分为 具名和作用域插槽

- 具名插槽：当组件内有多处标签需要自定义时，传入的标签可以派发给不同的 `slot` 位置

```js
// 组件内用 slot 占位 配上 name 属性，使用时 template 配合v-slot：name 值传入具体标签
// v-slot 可以简写成 #
<slot name="title"></slot>
<slot name="content"></slot>
```

- 作用域插槽：当需要使用到子组件内的变量时 需要用到作用域插槽。

```js
// 1. 子组件内通过 slot占位 同时绑定属性和子组件的值 
 <slot :row="defaultObj">{{ defaultObj.name }}</slot>
// 2. 使用组件 传入自定义标签,用 template 和 v-slot='自定义变量名'
// 3. scope 变量名自动绑定 slot 上所有属性和值
 <template v-slot="scope">
     	// scope: {row: defaultObj} scope.row 就是子组件绑定的变量值
          <p>{{ scope.row.name }}</p>
 </template>
```

### 15.路由

路由是一种映射关系，在 Vue 中路由是路径和组件的映射关系。由于 vue 是单页面应用，所以依赖于路由切换显示不同的页面。vue-router 是官方推荐的路由模块包，引入项目中即可使用

- SPA：指单页面应用，只有一个 html 文件，不用刷新页面用户体验好。但其缺点就是首次加载慢，不利于 SEO 优化
- 组件分类： 分为页面组件 `src/views\/pages` 和复用组件 `src/components` 
- 使用路由需先配置，后续使用脚手架都配置好了

```js
// 1. 下包 yarn add vue-router
// 2.导入 import VueRouter from 'vue-router'
// 3.导入组件对象 import My from './my.vue‘
// 4.注册路由 Vue.use(VueRouter)
// 5.配置规则 const routes = [{
      path: '/my', component: My , name: 'my', 
      chlidren: [
           { 路由规则}
      ]
 }]
// 5. 创建路由实例对象  const router = new VueRouter({
   routers, 
   mode: 'hash' // 路由模式 hash/history
})
// 6.挂载到 vue 实例上  new Vue({
  render: (h) => h(App),
  router,
}).$mount("#app");
// 7. 使用 router-view 组件占位, 将来路径匹配后会将指定的组件替换到此处
```

- 路由规则配置

| 配置      | 作用                             | 说明                        |
| --------- | -------------------------------- | --------------------------- |
| path      | 匹配路由访问的路径               |                             |
| component | 访问路径所对应的组件对象         | 可以使用懒加载              |
| redirect  | 路由重定向                       | 可以重定向到别的路径        |
| children  | 路由的嵌套，子路由               | 子路由 path 不带 /          |
| *         | 通配符，当所有的规则都匹配不上时 | 通常用来匹配 404 页面       |
| name      | 路由组件名字                     |                             |
| props     | 是否开启 props 传参              | 值为 true 时开启 props 传参 |

- 路由模式：分为 hash 模式和 history 模式，默认是 hash 模式，在实例化路由对象时修改

  ```js
  // history 模式打包上线后需要后台支持, 默认模式是 hash
  // hash 模式 url 地址栏后面带 #
  const router = new VueRouter({
    routes,
    mode: "history" 
  })
  ```

- 路由守卫：路由有三类钩子函数，最常用的就是前置导航守卫

```js
// 全局钩子函数，路由在跳转前会执行 beforeEach(to, from, next) => {}）
// to	要跳转的路由	from	从哪里跳转来的	next	执行 next() 路由才会正常跳转
// 单独路由独享组件	beforeEnter
// 组件内钩子	beforeRouterEnter beforeRouterUpdate beforeRouterLeave
```

- 路由跳转方式，分为声明式导航(链接跳转)和编程式导航( js 跳转)

  - 声明式导航：路由提供了一个全局组件` router-link` ，最终会渲染成 a 链接 to 属性等价于 href属性(to无需#)，且提供了导航高亮的功能(自带类名)

  ```js
  <router-link to="/find?name=张三&age=18">发现音乐</router-link>
  // 携带类名 router-link-active 模糊匹配	router-link-exact- active 精确匹配
  // 传参方式为 query 用?分隔 & 符号拼接	或者是 params	直接用 /name/age 表示
  // 接收参数 $route.query/$route.params 其中 params 需在路由规则中按顺序加 ：才会生效
  // query 参数称为查询参数	 params 参数称为路径参数，使用路径传参时还可以使用 props 接收参数
  // 当组件开启了 props 传参时，在组件内直接使用 props 接收参数即可代替 $route.params
  props:['name','age'] === $route.params.name/age
  // path 是路径部分，不包含参数		fullPath 是完整地址，包含参数
  ```

  - 编程式导航：用 js 来进行路由跳转 `$router.push`

  ```js
  // $router.push() 路由跳转(有历史记录) $router.replace() 路由替换（无历史记录）      $router.go() 路由前进(正值)/后退(负值)，超过最大值则原地不动。实际开发用		      $router.back() 后退 $router.forword() 前进
  this.$router.push({
    path: '/my',  // 路由路径
    name: 'my',	// 路由名
    query: {		// 使用 path 或 name 均可以
      name: 'zs'
    },
    params: {		// 使用 path 会忽略 params 的参数
     age: 18
    }
  })
  // 对应路由接收   $route.params.参数名  / $route.query.参数名  
  // 推荐用 path+query 传 $route.query 接或 name + params 传 $route.params 接
  // 使用 path + query 传参会在地址栏显示传的参数  name + params 传不会显示参数
  ```

- $route是路由参数对象，包括 ‘path，hash，query，fullPath，matched，name’ 等路由信息参数。

- $router是路由实例对象，包括了路由的跳转方法，实例对象等

- 路由工作原理：用户点击跳转，导致 url 地址栏的 hash 值发生变化，监听 hash 值的变化，触发 `window.onhashchange` 事件，使用动态组件将 hash 值对应的组件渲染到页面中。

### 16. Vuex

Vuex 是官方推荐的状态管理工具，采用集中式存储来管理应用所有组件的状态，解决多组件之间的数据通讯问题，简单来说就是独立于组件外的管理公共数据的工具。

总体来说 Vuex 分为五个部分，其中 state 和 mutations 是比较重要的

##### 1.state

- ##### 用来定义和存放公共数据，是一个对象

```js
// 定义数据，是一个对象，也可以写成一个函数，数据是响应式的，且不能直接修改数据
state: {
   属性名： 属性值 
 }

state：()=> ({ list: []})
// 组件中用 this.$store.state 来访问
```

##### 2.mutations

- #####  用来处理 state 中定义的数据，处理同步任务，处理异步任务不报错但是追踪不到数据的变化，且严格模式下会报错

```js
// mutation 是一个对象，每一项都是函数，有两个形参，第一个形参永远是 state ，第二个是载荷，也就是传递的参数，可选参数，如有多个参数需要传递可以写成对象
mutations: {
    函数名(state, payload) {
      state.list = payload;
 }}
// 组件中用 this.$store.commit('mutation名', 载荷) 来访问
```

##### 3.actions

- ##### 用来处理 Vuex 中的异步任务，通过调用 mutations 中的方法来间接修改 state 数据

```
//
actions:{
	// 第一个参数为 context 自动传入表示 store 实例，第二个参数为载荷，传入的参数
	函数名(context，载荷){
		函数体
		 context.commit('mutation名', 载荷)
	}}
// 组件中用 this.$store.dispatch('actions 名', 载荷) 来访问
```

##### 4.getters

- ##### 类似于组件中的计算属性。定义数据，对 state 中的数据进行计算得到新数据

```js
// 只能获取结果，没有 set 方法修改 state 数据
getters: {
   // 第一个参数永远是 state ，可以进行解构
  isShow({ list }) {
    return list.every((item) => item.goods_state === true);
 }}
// 组件中用 this.$store.getters 来访问
```

##### 5.modules

- ##### 拆分模块，将不同的场景按模块区分开，方便开发和维护

```js
// 使用需先在子模块中开设命名空间	namespaced: true
// 之后在主模块中导入子模块并注册 modules:{ 注册模块名：模块对象 }
// 使用子模块的数据和方法 $store.state.模块名.属性名	$store.getters.模块名.属性名
// $store.commit('模块名/函数名', 载荷) $store.dispatch('模块名/函数名', 载荷)
const store = new Vuex.Store({
  modules: {
   注册模块名：模块对象
});
```

##### 6.辅助函数

- ##### 是 Vuex 定义好的工具函数，可以优化访问 Vuex 中的属性和方法

```js
// 导入辅助函数 导入后可以直接使用
import { mapState, mapMutations, mapActions, mapGetters} from 'vuex'
// 优化 state 和 getters ，在组件 computed 属性中展开
computed: {
  ...mapState('模块名', ['属性名'] ), 
  ...mapGetters('模块名',['属性名'])
}
// 优化 mutations 和 actions ，在组件 methods 属性中展开
methods: { 
  ...mapMutations('模块名', ['xxx']), 
  ...mapActions('模块名',{'新名字': 'xxx'})
}
```

### 17.常见 UI 组件库

pc 端常用的 element-ui、ant、iview 等，移动端常用的 vant、didi、jd 等，小程序常用的 uniapp，配置及文档详见官网。

### 18.过滤器

> 这个功能在 Vue3 中被取消了

主要用来对插值表达式和 v-bind 的值进行处理，是一个函数

```js
// 全局过滤器，在 main.js 中定义,第一个参数是过滤器名，第二个参数是回调函数，形参就是原内容
Vue.filter('过滤器名'，function(val){
	// 此处的 val 就是过滤前的数据，进行一系列处理后 return 返回
})
// 局部过滤器，在组件内部定义，参数是一样，必须有返回值
filters: {
	过滤器名(形参){
		处理函数
	}
}
// 使用都是一样的，在数据后面用 | 隔开，后面写过滤器名
{{ msg | 过滤器名 }} 或 <div :list='list | 过滤器名'> 内容</div>
```

### 19.样式穿透

当组件 style 标签添加了 scoped 属性后，组件的每一个标签都会在原有的类名上自动添加属性选择器

```js
<style lang="less" scoped>
// 添加 scoped 后标签名变成了 p[data-v-sd5dsa] 这种原有选择器和属性选择器的结合
// 当组件内部使用了子组件时，直接修改子组件样式不会生效，因为子组件并没有添加上属性选择器
// 在子组件类名前添加穿透命令，即可将子组件类名变成 [data-v-sd5dsa] p 这种后代选择器，样式就会生效，在第三方的 UI 组件库中使用的较多
// less 中用 /deep/ 设置	scss 中用 ::v-deep 设置		css 中用 >>> 设置
 p {
  padding: 46px 0 50px 0;
}
</style>
```

### 20.EventBus 组件传值

> 这个功能在 Vue3 中被取消了

任意两个组件之间(兄弟组件或无任何关系的组件之间)传值可以使用 `EventBus`和 `Vuex` 来实现

有两种方式来创建

```js
// 1.创建 EventBus 的 js 文件，导出 vue 实例对象
import Vue from "vue";
// 导出空白vue对象
export default new Vue();
// 2.在 main.js 中原型上添加 EventBus
Vue.prototype.$EventBus = new Vue()
// 创建后需要传值时导入 EventBus ，当 A 向 B 传值时，A 组件中声明方法触发 EventBus 事件
 EventBus.$emit("自定义事件名", 实参/载荷);
// B 组件在 created 接收值，使用完后最好对 EventBus 进行销毁，防止全局污染
 created() {
    bus.$on("自定义事件名", (形参) => {})
  },
```

### 21. 依赖注入

通常使用 props 进行父子传值,但当组件嵌套较深时数据的传递会非常繁琐,官方提供了 `provide` 和 `inject` 方法方便祖先组件向其所有子孙后代传递数据,不论组件层级有多深均可实现

```js
 // 和 data methods 等平级,可以是一个对象或返回一个对象的函数
 provide: { foo: 'bar' }
 provide() {
    return { name: "张三", age: 18 };
 }
// 子孙后代组件接收传递的数据,使用 inject 接收, 可以是一个字符串数组,也可以是一个对象
inject: ["name", "age"],
inject: {
    foo: {
      from: 'bar', // from 表示数据的来源组件
      default: 'foo' // 与 prop 的默认值类似, 可以是一个默认值或者工厂函数
      default: () => [1, 2, 3]
    }
  }
```

