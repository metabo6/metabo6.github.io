---
title: NODE 常见第三方包
tags: NODE
abbrlink: 8152942f
date: 2022-11-22 11:51:32
---

###  NODE 常见第三方包

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

- Day.js 是一个轻量的处理时间和日期的 JavaScript 库，和 Moment.js 的 API 设计保持完全一样。 [官网文档](https://www.npmjs.com/package/dayjs)

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

-  Express 是一个第三方模块，用于快速搭建服务器（替代http模块）

使用Express构建Web服务器步骤：

```js
// 1) 加载 express 模块
const express = require('express');
// 2) 创建 express 服务器
const app = express();
// 3) 开启服务器
app.listen(3006, () => console.log('express 服务器开始工作了'));
// 4) 监听浏览器请求并进行处理
app.get('GET请求的地址', 处理函数);
app.post('POST请求的地址', 处理函数);
// send 方法会自动设置响应头；并且会自动把对象转成 JSON 字符串
res.send({ status: 0, message: '注册成功' })

// 解析 post 请求体参数: a=1&b=2&c=3        头信息：application/x-www-form-urlencoded
app.use(express.urlencoded({ extended: true }))
// 解析 post 请求体参数: '{"a":1,"b":2}'    头信息：application/json
app.use(express.json())
```

### 8. utility

- 数据库存储密码一般需要加密 常见加密方式 md5

```js
// 下载安装第三方加密模块，并解构里面的 md5 方法
  let { md5 } = require('utility') 
// 对密码进行加密
  password = md5(password)
```

### 9.express-jwt

- 该模块是专门配合express使用的进行身份验证的模块

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

```js
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

