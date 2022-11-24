---
title: Node 基础总结
tags: Node
abbrlink: f2528fa6
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/nodejs.webp
---



## node 基础学习总结

### 1. 概念

- node.js 是基于 chrome v8 引擎构建的 js 运行环境,让 js 有了做后端开发的能力

### 2. 模块化及分类

- 模块化,就是把大的文件拆分成一个个小的文件,而且能通过特定语法组合到一起的实现过程
- 更利于项目的维护,且具有更好的复用性
- 分为 自定义模块  内置模块    第三方模块

### 3. 内置模块

1. #### path 模块

   ```
   // 常见内置 api
   path.basename(path[, ext]) // 返回 path 的最后一部分(文件名) 
     path.basename('./aaa/bbb/index.html') //  index.html
   path.dirname(path)        // 返回目录名 
     path.dirname('./aaa/bbb/index.html')   // ./aaa/bbb
   path.extname(path)        //返回路径中文件的扩展名(包含.)  
     path.extname('./aaa/bbb/index.html') // .html
   path.format(pathObject)     将一个对象格式化为一个路径字符串 
   // 用法较为复杂 详见 https://vimsky.com/examples/usage/node-js-path-format-method.html
   path.join([...paths])     // 拼接路径 参数可以是多个,会从前往后依次拼接 
    path.join('aaa', 'bbb', 'index.js') // aaa/bbb/index.js
    //  __dirname 永远表示当前js文件的绝对路径
     path.join(__dirname, 'aaa/index.html') //D:\web\前端学习资料\11-node\bbb\index.html 
   path.parse(path)           // 把路径字符串解析成对象的格式 同 format 方法相反 
     path.parse('aaa/bbb/index.html') // {
        root: '', dir: 'aaa/bbb',base: 'index.html',ext: '.html',name: 'index'}
   path.resolve([...paths])   // 基于当前工作目录拼接路径   
      path.resolve('aaa/bbb/index.html') // D:\web\前端学习资料\11-node\aaa\bbb\index.html
   ```

   

2. #### fs 模块

   - 即 file system，文件系统，该模块可以实现对 文件、文件夹的操作, 文件操作都为异步操作

   ```
   // fs.access(path, callback)        判断路径是否存在
   // fs.appendFile(file, data, callback)     向文件中追加内容
   // fs.copyFile(src, callback)         复制文件
   // fs.mkdir(path, callback)          创建目录
   // fs.readDir(path, callback)       读取目录列表
   // fs.rename(oldPath, newPath, callback)       重命名文件/目录
   // fs.rmdir(path, callback)           删除目录          | 只能删除空目录 |
   // fs.stat(path, callback)            获取文件/目录信息
   // fs.unlink(path, callback)          删除文件
   // fs.watch(filename[, options]\[, listener])   监视文件/目录
   // fs.watchFile(filename[, options], listener) 监视文件
   // fs.readFile(path, callback)      异步读取文件 
   // fs.writeFile(path, callback)      异步读取文件
   // callback 一般包含两个参数 err , data 当读取文件错误时 err 是一个错误对象,data 为 undefined 读取成功时 err 为 null, data 为读取到的文件对象
   ```

   

3. #### querystring 模块

   - 该模块是老版本的用于解析和格式化网址查询字符串的方法

   - 官方推荐使用新的 api  `urlSearchParams`

   ```
   // querystring.parse() 将查询字符串解析成JS对象
   querystring.parse('id=1&name=zs&age=20') // { id: '1', name: 'zs', age: '20' }
   // querystring.stringify() 将JS对象转成查询字符串
   querystring.stringify({id:'1',name:'zs',age:'20'}) // id=1&name=zs&age=20
   ```

4. #### http 模块

   -  该模块是一个系统模块，能够通过简单的流程创建一个Web服务器

```
// 1. 加载http模块
const http = require('http')
// 2. 创建服务对象，一般命名为server
const server = http.createServer() // create创建、server服务器
// 3. 给server对象注册请求（request）事件，监听浏览器的请求。只要有浏览器的请求，就会触发该事件
server.on('request', (req, res) => {
	// 设置响应状态码
	res.statusCode = 200
	// 设置响应头
	res.setHeader('Content-Type', 'text/plain; charset=utf-8')
	// 设置响应体
	res.end('hello,欢迎访问服务器,这是服务器给你的回应')
})
// 4. 设置端口，开启服务
server.listen(3000, () => {
	console.log('服务器启动了')
})
// 代码的格式基本固定。只有 请求事件 的处理函数需要单独设置
```



### 3. Express 模块的使用

- express 是一个第三方模块，用于快速搭建服务器（替代http模块）
- 保留了http模块的基本API，使用express的时候，也能使用http的API ,如 res.end()/req.url
- 提供中间件功能
- 封装了新方法,能更方便搭建服务器

```
// 使用express 搭建web服务器
// 1) 加载 express 模块
const express = require('express');
// 2) 创建 express 服务器
const app = express();
// 3) 开启服务器
app.listen(3006, () => console.log('express服务器开始工作了'));
// 4) 监听浏览器请求并进行处理
app.get('GET请求的地址', 处理函数);
app.post('POST请求的地址', 处理函数);
app.get('/api/test', (req, res) => {
  // res.end('hello world，哈哈哈'); // 响应中文会乱码，必须自己加响应头
  // res.end(JSON.stringify({ status:0,message:'注册成功'})); // 只能响应字符串或者buffer类型
  // express提供的send方法，可以解决上面的两个问题
  res.send({status:0,message:'注册成功'})// send方法会自动设置响应头.并且会自动把对象转成JSON字符串
});
```

- #### express 路由 

  - 请求和处理程序的映射关系,使用路由可以降低匹配次数,提高性能 还能分类管理接口,便于维护和升级
  - 使用步骤如下 详见官网

  ~~~
  // 1. 加载express
  const express = require('express');
  // 2. 创建路由对象，实则是个函数类型
  const router = express.Router();
  // 3. 写接口，把接口挂载到 router 上
  router.post('/reguser', (req, res) => {});
  router.post('/login', (req, res) => {});
  // 一定要导出 router
  module.exports = router;
  ~~~

  在 index.js 中导入路由

  ```
  const express = require('express')
  const app = express()
  app.listen(3006, () => console.log('启动了'))
  // 加载自定义的路由模块，注册中间件
  let loginRouter = require('./routers/login')
  app.use('/api', loginRouter)
  
  ```

  请求参数的语法

  ```js
  // get 请求参数
  // 客户端请求: /api/list?id=2&name=zs
  // 接口写法: app.get('api/list',(req,res)=>{}) // 用 req.query 接收参数
  // 客户端请求: /api/list/id/name
  // 接口写法: app.get('api/list/:id/:name',(req,res)=>{}) // 用 req.params 接收参数
  // 中间件: app.use(express.json()) // 设置可以接收 json 类型的参数
  
  // post 请求参数
  // 客户端Content-Type: application/x-www-form-urlencoded
  // 中间件: app.use(express.urlencoded({ extended: true}))
  // 接口写法: app.post('api/list',(req,res)=>{}) // 用 req.body 接受参数
  // 客户端Content-Type: multipart/form-data
  // 加载 multer 模块: const multer = require('multer')
  // 配置上传目录: const upload = multer({ dest: 'uploads/'})
  // 接口写法: app.post('api/list',upload.single('field'),(req,res)=>{}) 
  // 用 req.body | req.file 接受参数
  ```

  

### 4. 导入导出总结

```
// commonJS 导入导出
module.exports = {}  // 默认导出
export.fn = ()=> {}  // 按需导出
const fn = require('./fn.js')  // 默认导入
const moduleNames = ['foo.js', 'bar.js']
moduleNames.forEach(name => {
  require('./' + name)
})     	// 动态加载指定模块
let module = {
  exports: {}
}
// 内在机制是将exports指向了module.exports
let exports = module.exports

// ES6 导入导出
export default {}  // 默认导出
export const fn = ()=>{}  // 按需导出
import fn from './fn.js'  // 默认导入  可以任意起变量名接受
impor { fn as fn1} from './fn.js'  // 按需导入 不能直接修改导入变量名 可以用 as 关键字修改

```



### 5. 中间件的概念和使用

- 中间件(Middleware )，特指业务流程的中间处理环节,本质就是一个函数

- 中间件函数中有四个基本参数， err、req、res、next

  -  如果写两个参数，那么两个参数肯定是 req 和 res
  -  如果写三个参数，那么三个参数肯定是 req, res 和 next
  -  如果写四个参数，这个中间件叫做错误处理中间件。一般放在所有接口的最后面

- 所有请求都可以进入使用`app.use()`注册的中间件，但要注意前缀

- 中间件分类可以分为五类

  -  应用级别的中间件（***index.js 中的中间件，全局有效*** ）
  -  路由级别的中间件（***路由文件中的中间件，只在当前路由文件中有效***）
  -  错误处理中间件（***四个参数都要填，一般放到最后***）
  - 内置中间件（express自带的，比如 `express.urlencoded({ extended: true })`）
  - 第三方中间件（比如multer、express-jwt、express-session、....）
  - 跨域问题可以使用中间件 cors 解决,两行代码搞定

  ```
  安装 npm i cors
  加载 const cors = require('cors');
  使用 app.use(cors());   // 注意，必须把这个中间件，放到最前面。
  ```

  

### 6. npm 发包

- 1. 注册 npm 账号 [官网](https://www.npmjs.com/)
  2. 终端切换镜像源到 npm 主站 执行 ` npm login` 
  3. 依次输入账号 密码(不可见) 邮箱 
  4. 运行 ` npm publish `命令, 即可将包发布到 npm 
  5. 如需要删除包,运行 ` npm unpublish 包名 --force `命令