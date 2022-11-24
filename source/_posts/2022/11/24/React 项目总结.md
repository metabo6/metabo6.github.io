---
title: React 项目总结
tags: React项目
abbrlink: 206a9599
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/react.webp
---



# React 项目总结

- 该项目使用 react、axios、react-router-dom、sass、antd、react-quill 、Eslint 等

## antd 使用

- antd  是用于研发企业中后台产品的 React UI 库
- 使用步骤：下包 => 导入样式文件 => 组件内引入使用
- 使用小结：
  - Button 组件通过 `htmlType` 属性设置为 submit 按钮
  - Button 组件可以配合 useState 设置 loading 状态效果
  - 表单组件中每项使用  `block` 属性设置 Input/Button 大小
  - 表单校验需给 `Form` 设置 `validateTrigger`,给 Form.Item 设置 `name` 和 `rules` 属性
  - 自定义校验规则，给 `rules` 属性设置 `validator` 函数， 值为 true 表示通过
  - 收集表单数据：给 `Form` 添加 `onFinish`，给 `Form.Item` 添加 `name`，给 `htmlType`设为 `submit`
  - 表单默认值：使用 ` initalValues` 设置，只有初始化以及重置时生效
  - `message` 提示组件，有三个参数：提示内容，关闭时间，关闭触发的回调函数
  - `Menu ` 菜单组件 `theme` 属性表示风格主题，`defaultSelectedKeys` 表示默认选中，配合 `location.pathname` 可实现菜单栏跟随路由路径高亮
  - 气泡确认框包括 `title`、`okText`、`cancelText`、`onConfirm` 等属性
  - `card` 组件有 `title ` 等属性
  - `Breadcrumb` 组件内部可配合 `Link` 组件实现面包屑跳转
  - `DatePicker` 组件日期格式是 moment 格式的，转换格式需注意
  - 日期国际化：导入需转换的语言包和对应样式文件，使用 `ConfigProvider` 组件传入  `locale` 语言包即可
  - `Table` 组件有 `dataSource ` 和 `columns` 属性，分别表示数据和表格单元格格式。如果数组数据对象中没有 key 属性，必须通过 `rowKey` 进行自行指定
  - 表格组件 `columns` 属性可以通过 render 方法渲染操作列，还可以进行判断，根据不同条件渲染不同内容
  - `pagination` 组件有 `current`、`pageSize`、`total`、`onChange` 等属性
  - 如果组件被包在 Form.Item 中，则会自动传入受控属性 `value` 和 `onChange` 事件。组件内可接收到
  - `Upload` 组件有 `fileList`、`action`、`name `、`onChange`等属性。
  - 手动触发表单项的校验可使用表单 .validateFields(['属性名'])
  - 动态给表单设置数据，需调用表单实例方法：`setFieldsValue({属性名：属性值,...})`
  - 图片预览功能需要 `upload` 组件提供 `onPreview`





## jsconfig.json

- 该文件位于项目根目录，是 js 项目的根目录，用来指定 js 项目的选项

```json
// 添加如下配置能让 IDE 识别 @ 路径并给出提示
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}

```



## craco

> 使用 CRA 创建项目会将所有配置信息隐藏在 react-scripts 包中，想修改配置就需通过第三方库

- craco 是用来对 CRA 创建的项目进行自定义配置的工具
- 使用步骤如下

```js
// 1. 下包 yarn add @craco/craco -D
// 2. 创建配置文件 craco.config.js，在该配置文件内即可自定义配置

// 配置路径别名：脚手架工具默认不能识别 @ 符号
const path = require('path')
module.exports = {
  webpack: {
    alias: {
      '@': path.join(__dirname, 'src')
    }
  }，
// 这里补充style配置
  style: {
    // postcssOptions 取代了原来的 postcss
    postcssOptions: {
      plugins: [vw]
    }
  },
}

// 3.修改 package.json 脚本命令
"scripts": {
  "start": "craco start",
  "build": "craco build",
  "test": "craco test",
  "eject": "react-scripts eject"
}

// 配置完毕后重启项目让配置生效，就可以在项目中使用 @ 符号
```



## axios 

- 在 `utils/request.js` 中做基本配置，包括项目基地址、请求/响应拦截器。
- 请求拦截器根据 token 判断来实现其他请求需携带 token 的需求
- 通过响应拦截器处理 token 失效，重定向至登录页。
- 跳转登录页在非组件内无法直接使用 `history` 跳转。安装 react-router-dom 时会默认安装 history 包，通过这个包在 utils 目录下封装自定义的 `history` 对象



## sass

- react 脚手架中已有 sass 配置，只需安装依赖包即可使用

```js
// 1. 下包 yarn add sass -D
// 2. 将 index.css 改为 index.sass 文件
// 3. 使用 sass 导入绝对路径时需加上 ~
```



## css 样式污染

- 项目中如果组件之间选择器有重复，会导致一个组件样式在其他组件中生效造成样式污染
- 解决方法： `css in js` ，是使用 js 编写 css 的统称，用来解决样式冲突、污染的问题
- `css in js` 有多种实现方式，更推荐 CSS Modules 方式(React脚手架已集成，可直接使用)



## CSS Modules

- 该包已在脚手架中集成，可直接使用，用于解决 CSS 样式冲突和覆盖的问题

- 原理：通过自动给 CSS 类名补足类名，保证类名的唯一性，从而避免样式冲突的问题 

- 注意点：

  - 类名最好使用驼峰命名，因为最终类名会生成 `styles` 的一个属性

  ```js
  .tabBar {}  ---> styles.tabBar
  ```

  - 如果使用中划线的命名，需要使用[]语法 

  ```js
  .tab-bar {}  ---> styles['tab-bar']
  ```

- 步骤如下

```js
// 1.改样式文件名，从 `xx.scss -> xx.module.scss`
// 2.组件中导入该样式文件（注意语法）
import styles from './index.module.scss'

// 3.通过 styles 对象访问对象中的样式名来设置样式
<div className={styles.css类名}></div>
```

- 维持类名：使用 global 关键字来维持原类名，`:global(.a) {   }`
- 更推荐组件根组件使用  CSSModules 形式的类名，其他子节点使用普通 CSS 类名  :global

```jsx
// 根组件使用 CSSModules 形式
.root {
  display: 'block';
  position: 'absolute';
 //  子节点使用普通 CSS 类名，使用 :global 包裹
  :global {
    .title {
      .text {
      }
      span {
      }
    }
    .login-form { ... }
  }
}

import styles from './index.module.scss'
const 组件 = () => {
  return (
      // 组件内根节点使用 styles.root
  	<div className={styles.root}>
     //  所有子节点，都使用普通的 CSS 类名
    	<h1 className="title">
      	<span className="text">登录</span>  
      	<span>登录</span>  
      </h1>
			<form className="login-form"></form>
    </div>
  )
}
```



## 引入ESlint

- 安装并配置eslint，配置vscode快捷键,ctrl + s 保存格式化。

[查看详细配置](https://blog.csdn.net/weixin_58207509/article/details/121171835?spm=1001.2014.3001.5502)



## Git 管理项目

- 随着项目的进展，使用 git 同步更新进度



## 配置Redux

- 完成 `Redux` 的基础配置
- 步骤如下：

```js
// 1. 下包 yarn add redux react-redux redux-thunk redux-devtools-extension
// 2. 创建 actions、reducers、store 相关目录，并注册中间件
// 3. 在 src/index.js 中为 React 组件接入 Redux
```



## token 存储

- 在 `utils` 目录下创建 storage 文件，封装相关函数



## 路由权限控制

- 项目中有的页面需要 **登录** 后才能访问，而 react 中没有导航守卫。故需要自己封装`AuthRoute` 组件来做权限控制，在该组件内判断是否有 token
- 因 **token 失效**、**非法访问**、**用户退出**等情况而跳入登录页面，正常登录之后再跳转回之前访问的页面。在跳转回登录前使用 state 记录当前页信息，方便登录后跳转回



## 项目本地预览

**目标**：能够在本地预览打包后的项目
**步骤**：

1. 全局安装本地服务包：`npm i -g serve`，该包提供了 `serve` 命令，用来启动本地服务
2. 在项目根目录中执行命令：`serve -s ./build`，在 build 目录中开启服务器
3. 在浏览器中访问：`http://localhost:5000/` 预览项目



## 打包体积分析

**目标**： 安装 `source-map-explorer`包来分析打包体积，优化项目体积

**步骤**：

1. 安装分析打包体积的包：`npm i source-map-explorer`

2. 在 package.json 中的 scripts 标签中，添加分析打包体积的命令

   ```json
   "scripts": {
     "analyze": "source-map-explorer 'build/static/js/*.js'",
   }
   ```

3. 对项目打包：`npm run build`（如果已经打过包，可省略这一步）

4. 运行分析命令：`npm run analyze`

5. 通过浏览器打开的页面，分析图表中的包体积



## 路由懒加载

**目标**：使用 react 提供的方法对路由进行懒加载实现代码分隔

**步骤**

1. 在 App 组件中，导入 Suspense 组件和 lazy 方法
2. 在 Router 内部，使用 Suspense 组件包裹组件内容
3. 为 Suspense 组件提供 fallback 属性，指定 loading 占位内容
4. 使用 lazy 函数，修改为懒加载方式导入路由组件



## 去掉 console

**目标**：使用 `terser-webpack-plugin` 包去除生产环境下的所有控制台打印

```js
// 下包 yarn add terser-webpack-plugin -D
const TerserPlugin = require('terser-webpack-plugin')

module.exports = {
  webpack: {
    // 省略其他...
		plugins: [
      new TerserPlugin({
        terserOptions: {
          compress: {
            drop_console: process.env.NODE_ENV === 'production' 
            // 生产环境下移除控制台所有的内容
          }
        }
      })
    ]
  }
}
```



## 配置CDN

**目标**：通过 craco 来修改 webpack 配置，从而实现 CDN 优化

```js
// 在 craco.config.js 中做如下配置
const path = require('path')
const { whenProd, getPlugin, pluginByName } = require('@craco/craco')
module.exports = {
  webpack: {
    configure: (webpackConfig) => {
      let cdn = {
        js: [],
        css: []
      }
      // 对webpack进行配置
      whenProd(() => {
        // 只会在生产环境执行
        webpackConfig.externals = {
          react: 'React',
          'react-dom': 'ReactDOM',
          redux: 'Redux',
        }

        cdn = {
        js: [           'https://cdn.bootcdn.net/ajax/libs/react/17.0.2/umd/react.production.min.js',
            'https://cdn.bootcdn.net/ajax/libs/react-dom/17.0.2/umd/react-dom.production.min.js',
            'https://cdn.bootcdn.net/ajax/libs/redux/4.1.0/redux.min.js'
          ],
          css: []
        }
      })

      const { isFound, match } = getPlugin(
        webpackConfig,
        pluginByName('HtmlWebpackPlugin')
      )
      if (isFound) {
        // 找到了 HtmlWebpackPlugin 的插件
        // match.options.cdn = cdn  for webpack 4
        match.userOptions.cdn = cdn
      }
      return webpackConfig
    }
  }
}

// 在 public/index.html 中做配置
// css 优化
<% htmlWebpackPlugin.options.cdn.css.forEach(cdnURL => { %>
      <link rel="stylesheet" href="<%= cdnURL %>"></link>
<% }) %>
        
// js 包优化
<% htmlWebpackPlugin.options.cdn.js.forEach(cdnURL => { %>
      <script src="<%= cdnURL %>"></script>
<% }) %>        
```



