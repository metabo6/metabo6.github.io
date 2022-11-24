---
title: NODE常见第三方包
tags: Node
abbrlink: cac7ea72
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/npm.webp
---

## NODE 常见第三方包

### 1. MySQL 

#### 使用步骤 详见官网 

在Node中使用MySQL模块一共需要5个步骤：

1) 加载 MySQL 模块

2) 创建 MySQL 连接对象

3) 连接 MySQL 服务器

4) 执行SQL语句           

5) 关闭链接  

```js
// 1. 加载mysql模块
const mysql = require('mysql');
// 2. 创建连接对象（设置连接参数）
const conn = mysql.createConnection({
    // 属性：值
    host: 'localhost',
    user: 'root',
    password: '密码',
    database: '数据库名'
});

// 3. 连接到MySQL服务器
conn.connect();

// 4. 完成查询（增删改查）
// 4. 完成增删改查
let sql = 'select * from student where id<5';
let sql = 'insert into student set name="张三", age=20, sex="男"';
let sql = 'update student set age=30, sex="女" where id=3';
let sql = 'delete from student where id=3';
conn.query(sql, (err, result) => {
    err: 错误信息
    result: 查询结果
});

// 5. 关闭连接，释放资源
conn.end();
```



### 2.  moment 模块 

- moment是一个专门处理时间日期的模块 类似于 dayjs 

  ```js
  moment().format("YYYY-MM-DD HH:mm:ss")
  ```



### 3. dayjs 模块

- Day.js 是一个轻量的处理时间和日期的 JavaScript 库，和 Moment.js 的 API 设计保持完全一样 [](https://www.npmjs.com/package/dayjs)



 ### 4. **jsonwentoken**

- jsonwebtoken模块的作用是生成token字符串。

```js
// jwt.sign()
// 1. 参数1：对象，要在token保存的数据
// 2. 参数2：加密的字符串，类似于一个钥匙。随便填；后续解密token的时候，需要使用它
// 3. 参数3：对象，配置项，比如配置一下过期时间
// 4. 参数4：生成token后的回调函数
jwt.sign({ id: 1 }, 'abcde', { expiresIn: '2h' }, (err, result) => {
  if (err) throw err;
  console.log('Bearer ' + result);
});
```



### 5.**nodemon模块**

- 该模块可以代替 node 命令 ,修改文件后会自动重启服务 不必再每次修改后再执行 node 命令

```js
// 需要全局安装 npm i -g nodemon
nodemon ./index.js
```



### 6. nrm

- 该模块可以很方便的切换 npm 的镜像源

```js
// 需要全局安装 npm i -g nrm 
nrm ls    --- 查看全部可用的镜像源
nrm use taobao  ---- 切换到淘宝镜像
nrm use npm  ---- 切换到npm主站
```



### 7. Express

-  Express 是一个第三方模块，用于快速搭建服务器（替代 http 模块）

使用 Express 构建 Web 服务器步骤：

```js
// 1) 加载 express 模块
const express = require('express');
// 2) 创建 express 服务器
const app = express();
// 3) 开启服务器
app.listen(3006, () => console.log('express服务器开始工作了'));
// 4) 监听浏览器请求并进行处理
app.get('GET请求的地址', 处理函数);
app.post('POST请求的地址', 处理函数);
// 常用的 send 方法
res.send({ status: 0, message: '注册成功' }); // send方法会自动设置响应头；并且会自动把对象转成JSON字符串
// 解析 post 请求体参数: a=1&b=2&c=3        头信息：application/x-www-form-urlencoded
app.use(express.urlencoded({ extended: true }))
// 解析 post 请求体参数: '{"a":1,"b":2}'    头信息：application/json
app.use(express.json())
```



### 8. utility

- 数据库存储密码一般需要加密 常见加密方式 md5

```
// 下载安装第三方加密模块，并解构里面的 md5 方法
  let { md5 } = require('utility') 
// 对密码进行加密
  password = md5(password)
```



### 9.express-jwt

- 该模块是专门配合 express 使用的进行身份验证的模块

```js
const jwt = require('express-jwt');
// app.use(jwt().unless());
// jwt() 用于解析token，并将 token 中保存的数据 赋值给 req.auth
// unless() 完成身份认证
app.use(jwt({
  secret: 'abcde 生成token时的 钥匙，必须统一
  algorithms: ['HS256'] // 必填，加密算法，无需了解
}).unless({
  path: ['/api/login', '/api/reguser'] // 除了这两个接口，其他都需要认证
  { path: [/^\/api\//] })  // 还可以用正则表示 排除所有 api 开头的请求
}));
```



### 10. multer

- 该模块主要帮助服务端获取 FormDate 类型数据

```
const multer= require('multer')
const upload = multer({ desc: 'uploads/' })
router.post('/add', upload.single('cover_img'), (req, res) => {
  // upload.single() 方法用于处理单文件上传
  // cover_img 图片字段的名字
  
  // 通过 req.body 接收文本类型的请求体，比如 title,content等
  // 通过 req.file 获取上传文件信息
});
```



### 11.CORS

- CORS，叫做跨域资源共享，是XHR2.0中提出的一种新的解决跨域的方案。从IE10开始支持。
- CORS方案的实现，是通过服务器的响应头来实现的。
- 服务器要设置：`Access-Control-Allow-Origin: '*或者一个具体的源'`

```js
- 安装 npm i cors
- 加载 const cors = require('cors');
- 使用 app.use(cors());   // 注意，必须把这个中间件，放到最前面。这样即可解决跨域问题
```



### 12.then-fs

- 该模块可以将读写文件操作后返回 promise 对象,更方便的操作文件,不必再自己封装

~~~js
import fs from 'then-fs';

fs.readFile('./files/a.txt', 'utf-8')
  .then(res1 => {
    console.log(res1);
    return fs.readFile('./files/b.txt', 'utf-8')
  })

~~~



### 13. webpack-dev-server

- 该包创建一个服务器，监听指定目录下的文件，自动打包文件到内存上，代码变化只会更新变动的部分

```js
// 1. 下包	yarn add webpack-dev-server
// 2. 配置自定义命令 serve
scripts: {
	"build": "webpack",
	"serve": "webpack serve"
}
// 3. 行命令-启动webpack开发服务器	yarn serve
// 服务期配置，在 webpack.config.js 中配置 devServer
devServer: {
      port: 3000, // 端口号
      open: true // 启动后自动打开浏览器
 }
```



### 14. koa 框架

​	该框架是一个服务器框架，类似于 express 框架，可以在本地快速搭建服务器。

​	还可以在项目打包上线时进行线上测试

**下包**

```bash
npm install koa koa-static
```

**搭建服务器**

```js
const Koa  = require('koa')
const serve = require('koa-static')
// 处理 history 模式中间件 npm install koa2-connect-history-api-fallback
const { historyApiFallback } = require('koa2-connect-history-api-fallback');
// 处理跨域问题中间件 npm install koa2-proxy-middleware
const proxy = require('koa2-proxy-middleware')
const app = new Koa();

app.use(proxy({
  targets: {
    '/api/(.*)': {
        target: '后端服务器地址',
        changeOrigin: true
    }
  }
}))

// 这句话的意思是除接口之外所有的请求都发送给了 index.html
app.use(historyApiFallback({ 
  whiteList: ['/api']
}));  // 这里的 whiteList 是白名单的意思

app.use(serve(__dirname + "/public")); //将 public 下的代码静态化
app.listen(3333, () => {
  console.log('项目启动: 3333端口')
})
```







## vue 开发常见包



### 1. vuex-persistedstate

vuex 数据保存在内存中,刷新页面会丢失,借用该插件可以实现 vuex 数据本地持久化保存

```js
import createPersistedState from 'vuex-persistedstate' // 导入包
plugins: [
    createPersistedstate({
      key: 'xtx-pc-store', // key 表示存储的名字,可以自定义,最好语义化
      paths: ['user', 'cart'] // 有子模块需加入路径
    })
  ],
```



### 2. js-cookie

该插件是用来做 token 持久化处理，刷新页面之后token不丢失。

```js
import Cookies from 'js-cookie'
const TokenKey = 'hrsaas-ihrm-token' // 设定一个独一无二的key
export function getToken() {
  return Cookies.get(TokenKey)
}

export function setToken(token) {
  return Cookies.set(TokenKey, token)
}

export function removeToken() {
  return Cookies.remove(TokenKey)
}

```





### 3. style-resoures-loader

> less 变量自动导入

使用 less 开发时,一些变量单独存放在一个文件中,每次使用变量都需要先导入变量文件再使用,非常繁琐, 可以采用自动导入的方式,方便业务组件使用全局 less 变量

```js
// 首先准备变量文件, 如 src/styles.var.less 文件, 里面定义了很多变量,每次使用需先导入
@import "~@/styles/var.less";
.test {
  color: @xtxColor;
}
// 解决方案: 使用 vue-cli 的 style-resoures-loader 插件实现
// 执行命令添加插件 vue add style-resources-loader
// 选择 less 后会在 vue.config.js 中自动添加配置,修改好配置重启服务
const path = require('path')
 pluginOptions: {
    'style-resources-loader': {
      preProcessor: 'less',
      patterns: [
        // 配置哪些文件需要自动导入
        path.join(__dirname, './src/styles/var.less')
      ]
    }}
```



### 4.@vueuse/core 

>  吸顶头部交互

电商类网站首页都很长,需要滚动查看内容,滚动过程中提供一个吸顶的交互让用户方便跳转,

实现思路基于滚动事件, 不断获取当前距离顶部的距离,大于该距离组件显示,小于时隐藏

具体来说可以使用 vue 中动态类名来实现,先准备一个类名控制好样式和位置,监听滚动距离,大于时添加类名,小于时移除类名

```js
yarn add @vueuse/core 	// 安装该包,其封装了写常见的交互逻辑
// 详见官网	https://vueuse.org/core/useWindowScroll/
```



### 5. nprogress

在路由跳转时，在页面顶部补充一些进度条提示。一般用在路由跳转时和发请求时。

```js
import NProgress from 'nprogress' // progress bar
import 'nprogress/nprogress.css' // progress bar style


NProgress.start() // 开始
 NProgress.done() // 结束
```



### 6. xlsx

该包可以用来处理上传、下载 Excel 文件，项目中使用较多

在 `vue-element-admin` 项目中有封装好的组件和功能，可以自定义使用



### 7. cos-js-sdk-v5

​	该包是腾讯云官方 sdk 包，可以用来上传文件、照片、视频等

```js
// 下面的代码是固定写法
const COS = require('cos-js-sdk-v5')
// 填写自己腾讯云 cos 中的 key 和 id (密钥)
const cos = new COS({
  SecretId: 'xxx', // 身份识别 ID
  SecretKey: 'xxx' // 身份秘钥
})
cos.putObject(
          {
            Bucket: 'metabo6-1309623358' /* 存储桶 */,
            Region: 'ap-nanjing' /* 存储桶所在地域，必须字段 */,
            Key: file.name /* 文件名 */,
            StorageClass: 'STANDARD', // 上传模式, 标准模式
            Body: file, // 上传文件对象
            // 上传进度
            onProgress: function(progressData) {
              console.log(JSON.stringify(progressData))
              this.percentage = progressData.percent * 100
            }
          },
          // 回调函数
          (err, data) => {
            if (data.statusCode === 200) {
              // 如果成功 地址赋值给图片
              const urlImg = `https://${data.Location}`
              this.$emit('input', urlImg)
            }
          }
        )
```



### 8. antv-G2

数据可视化图标。使用方法大同小异，基本都是根据官方文档来配置

[官网示例](https://g2.antv.vision/zh)、[官网文档](https://g2.antv.vision/zh/docs/api/general/chart)



### 9. echarts

> 注意给当前组件根标签绑定 id 属性，设置宽高，否则不生效

​	封装组件来使用，导入 `echarts` 包，在挂载阶段初始化数据，

[官网示例](https://echarts.apache.org/zh/index.html)



### 10. vue-i18n

> 注意版本问题，新版本 api 有些不兼容

​	该包用来做多语言国际化支持，可以实现 elementUI 多语言切换

```bash
npm i vue-i18n@8.22.2
```

**配置如下**

```js
import Vue from 'vue' // 引入Vue
import VueI18n from 'vue-i18n' // 引入国际化的插件包
import locale from 'element-ui/lib/locale'
import elementEN from 'element-ui/lib/locale/lang/en' // 引入饿了么的英文包
import elementZH from 'element-ui/lib/locale/lang/zh-CN' // 引入饿了么的中文包
Vue.use(VueI18n) // 全局注册国际化包
// 创建国际化插件的实例
const i18n = new VueI18n({
  // 指定语言类型 zh表示中文  en表示英文
  locale: 'zh',
  // 将elementUI语言包加入到插件语言数据里
  messages: {
    // 英文环境下的语言数据
    en: {
      ...elementEN
    },
    // 中文环境下的语言数据
    zh: {
      ...elementZH
    }
  }
})
// 配置elementUI 语言转换关系
locale.i18n((key, value) => i18n.t(key, value))
export default i18n
```

**挂载 i18n 插件**

```js
// 省略其他...
import i18n from '@/lang'
// 加入到根实例配置项中
new Vue({
  el: '#app',
  router,
  store,
  i18n,
  render: h => h(App)
})
```



### 11. screenfull

​	浏览器自带 API 可以实现但有兼容性问题，使用该插件可以实现全屏功能，且解决兼容性问题

下包

```
npm i screenfull
```

**使用**

```js
import screenfull from 'screenfull'

// 自定义事件
<svg-icon
   icon-class="fullscreen"
   class="fullscreen"
+  @click="toggleScreen"
/>

toggleScreen () {
 // 包提供的方法 toggle() 切换全屏
   screenfull.toggle()
}


  	// ESC会改变当前的全屏状态
    // screenfull 是插件，它自带事件监听 on
    // screenfull.isFullscreen ：插件自带的属性，表示当前是否处于全屏状态
    screenfull.on('change', () => {
      console.log('当前是否是全屏', screenfull.isFullscreen)
      this.isFullScreen = screenfull.isFullscreen
    })

```







## react 开发常见包



### 1. redux-thunk

是一个中间件,对 redux 原有的功能进行拓展, 让 redux 可以处理函数形式的  action , 在函数形式的 action 中就可以执行异步代码,发请求等异步操作.

```js
// redux 处理同步对象任务
dispatch{type: 'todos/add', payload: '学习redux'}
// 使用 redux-thunk 中间件后执行异步任务
// 1. 安装包 yarn add redux-thunk  2. 在 store/index.js 中导入并注册
import { createStore, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'
const store = createStore(rootReducer, applyMiddleware(thunk))
// 3. 修改 action creator , 返回一个函数, 形参就是 redux 中的 dispatch
const addTodo = (name)=> {
  return async (dispatch) =>{
    const res = await 异步动作()
    dispatch({type: 'todos/add', payload: name})
  }
}
dispatch(addTodo('学习redux'))
```

redux 提供了 applyMiddleware 功能， 允许添加中间件. 



### 2. react-redux

react-redux 是官方提供的 react 绑定库, 由于 redux 是独立的库,并不专门用于 react, 所以有这个绑定库将 react 和 redux 绑定在一起.其目的是为 react 接入 redux, 实现使用 redux 进行状态管理. 

```js
// 在 react 中直接使用 redux 需要每个组件单独导入 store , 且在根组件上写法不友好
import {Provider,useDispatch, useSelector} from 'react-redux'
// 使用步骤: 1.下包 yarn add react-redux ; 2. 在根组件中引入 Provider 直接包裹根组件
<Provider store={store}>
    <App />
</Provider>
// 包裹后就不需要每个组件导入 store; 3.获取公共状态使用 useSelector 方法,代替了 store.getState(), state 变化后会自动更新.
const 状态 = useSelector(store的数据 => 你需要的部分)
const list = useSelector((state) => state.todos) // state 表示 store 中的数据
// 4. 更新修改数据使用 useDispatch 方法
const dispatch = useDispatch()
dispatch(action)
```

### 3. redux-logger

是一个 redux 中间件,可以方便查看 redux 操作日志, 使用时必须设置成最后一个中间件

```js
// 1.下包 yarn add redux-logger; 2.引入注册
import {createStore, applyMiddleware} from 'redux'
import thunk from 'redux-thunk'
import logger from 'redux-logger'
import rootReducer from './reducer'
export default createStore(rootReducer, applyMiddleware(thunk, logger))
// 注册后每个 store 操作日志均会在控制台打印
```



### 4. redux-devtools-extension

改名为 redux-devtools/extension.旧版本仍然有效.此包可以方便在项目中启用调试工具,通过开发者工具调试跟踪 redux 状态.

```js
// 1. 安装 react 和 redux 开发者工具(浏览器插件); 2. 下包 yarn add redux-devtools-extension ; 3. 导入和配置
import { createStore, applyMiddleware } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'
export default createStore(reducer, composeWithDevTools(applyMiddleware(thunk, logger 等中间件..)))
// 配置完毕后可在调试工具查看
```



### 5. json-server

- json-server 是一个能根据已有的 json 文件快速生成接口的包, 可以方便的自己定义接口

```js
// 1. 全局安装 npm i json-server -g
// 2. 项目根目录新建 mock-server/db.json 文件,里面内容是 JSON 格式的对象
{	// 此处的 assets 可以自定义,同接口地址一致
  "assets": [
	    { "id": 1, "name": "外套", "price": 99 },
	    { "id": 2, "name": "裤子", "price": 34 } 
	]
}
// 3. 启动服务 输入命令 json-server Mock-Serve/db.json --watch --port 3001
// Mock-Serve/db.json 表示文件路径 3001表示端口号
// 也可以在 package.json 中添加命令快速启动 启动完毕后会自动生成接口地址如下
```

| 接口地址                | 请求方式 | 操作类型                       |
| ----------------------- | -------- | ------------------------------ |
| localhost:3000/assets   | GET      | 获取全部数据                   |
| localhost:3000/assets/1 | GET      | 获取单个数据                   |
| localhost:3000/assets   | POST     | 添加操作 。参数： {name,price} |
| localhost:3000/assets/1 | DELETE   | 删除操作                       |
| localhost:3000/assets/1 | PUT      | 完整修改。参数：{name,price}   |
| localhost:3000/assets/1 | PATCH    | 局部修改。参数：{name}         |



### 6. eslint-plugin-react-hooks

- 该插件是开发依赖, 可以帮助检查 Hooks 的使用是否符合规则

```js
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



### 7. classnames

- 该包用于有条件地将类名连接在一起。该函数接受任意数量的参数，可以是字符串或对象


```js
// 下包： yarn add classnames
import classnames from 'classnames'

classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames({ 'foo-bar': true }); // => 'foo-bar'
classNames({ 'foo-bar': false }); // => ''
classNames({ foo: true }, { bar: true }); // => 'foo bar'
classNames({ foo: true, bar: true }); // => 'foo bar'

// lots of arguments of various types
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

// other falsy values are just ignored
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'
```



### 8. highlight.js

- 该包可实现 code 标签或代码块高亮的功能

```tsx
// 下包：yarn add highlight.js

import hljs from 'highlight.js'
import 'highlight.js/styles/vs2015.css'
import { useEffect } from 'react'
export default function Question () {
  useEffect(() => {
    // 配置 highlight.js
    hljs.configure({
      // 忽略未经转义的 HTML 字符
      ignoreUnescapedHTML: true
    })
    // 获取到内容中所有的code标签
    const codes = document.querySelectorAll('.dg-html pre code')
    codes.forEach((el) => {
      // 让code进行高亮
      hljs.highlightElement(el as HTMLElement)
    })
  }, [])
  const content = `<pre>
 	 <code>console.log(abc)</code>
  	<code>console.log(abc)</code>
    </pre>
    <h3>nihoa</h3>
    <pre>
      <code>console.log(abc);xxx.forEach(item=>{console.log(1)})</code>
    </pre>`

  return (
    <div className="dg-html">
      Question
      <div dangerouslySetInnerHTML={{ __html: content }} />
    </div>
  )
}

```



### 9. dompurify

- 该包可对 HTML 内容进行净化处理，有效防止 XSS 攻击

```tsx
// 下包：	yarn add dompurify @types/dompurify -D

import DOMPurify from 'dompurify'

<div
  className="content-html"
  dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(content) }}
  >
</div>
```





## TypeScript 开发常见包

### 1. typescript 

- 该包是 TS 官方编译工具包，全局安装后可以用来编译 TS 代码。 提供了 `tsc` 命令

```js
// 下载全局包 npm i -g typescript ||	yarn global add typescript

// 验证是否安装成功 tsc -v

// 1. 创建 ts 文件后 执行 tsc hello.ts，会在同级目录生成同名 JS 文件
// 2. 使用 node 执行 js 代码	node hello.js
// 3. 编译后的 js 文件没有类型信息

```



### 2. ts-node

- 该包是一个简化运行 TS 代码的包，可以实现直接在 Node.js 中执行 TS 代码

```js
// 下载全局包 npm i -g ts-node ||	yarn global add ts-node

// 安装后提供了 ts-node 命令，可以直接执行 ts 代码，但不会生成同名 js 文件

// 其命令相当于执行了 tsc 命令，再执行 node 命令

```

