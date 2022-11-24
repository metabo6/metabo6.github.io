---
title: React 移动端总结
tags: React 移动端
abbrlink: cd25c3da
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/react.webp
---



# React 移动端总结

该项目主要使用 react、redux、antd-mobile、axios、react-router、sass、ts、ahooks 库、lodash、websocket 等 



## antd-mobile

antd-mobile 是 Ant Design 的移动规范的 React 实现，服务于蚂蚁及口碑无线业务。

**使用**：

1. 下包

```
 yarn add antd-mobile
```

2. 按需导入即可使用

```tsx
import { Button } from 'antd-mobile'

const Login = () => (
  <div>
    <Button color="primary">Button</Button>
  </div>
)
```



### antd-mobile主题定制

[antd-mobile 主题](https://mobile.ant.design/zh/guide/theming)

src/index.scss 中：

```scss
:root:root {
  --adm-color-primary: #fc6627;
  --adm-color-success: #00b578;
  --adm-color-warning: #ff8f1f;
  --adm-color-danger: #ff3141;
  --adm-color-white: #ffffff;
  --adm-color-weak: #999999;
  --adm-color-light: #cccccc;
  --adm-border-color: #eeeeee;
  --adm-font-size-main: 13px;
  --adm-color-text: #333333;
}
```



## CSS 变量

[CSS变量-自定义属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_custom_properties)

CSS 自定义属性，通常称为 `CSS 变量`。类似于 JS 中声明的变量，可以**复用 CSS 属性**值。其变量有作用域之分。

**定义格式** ： `变量名：值` 

```css
/* 1 创建全局 CSS 变量 --geek-color-primary*/
:root {
  --geek-color-primary: #fc6627;
}
```

两个--开头

**使用格式**:  `属性名: val(变量名)`

示例

```js
/* 2 复用 */
.list-item-active {
  color: var(--geek-color-primary);
}
```



根据 CSS 变量的作用域，分为两种：

1. 全局 CSS 变量：全局有效
2. 局部 CSS 变量：只在某个作用域内（比如，某个类名中）有效

```css
/*
  全局 CSS 变量
  1. 使用 :root 这个 CSS 伪类匹配文档树的根元素 html。可以在 CSS 文件的任意位置使用该变量
     相当于 JS 变量中的全局
  2. CSS 变量通过两个减号（--）开头，多个单词之间推荐使用 - 链接。CSS 变量名可以是任意变量名
*/
:root {
  --geek-color-primary: #fc6627;
}
/* 使用 */
.tabs-item-active {
  color: var(--geek-color-primary);
}
.list-item-active {
  color: var(--geek-color-primary);
}

/* 
  局部 CSS 变量
*/
.list {
  --active-color: #1677ff;

  /* 在该 类 内部使用改变量 */
  color: var(--active-color);
}
.test {
  /* 这里的错误演示：无效！效果与不使用该变量时一致*/
  color: var(--active-color); 
}
```



## craco 

与 pc 项目基本一致，下包后可自定义项目配置

1. 下包

```js
yarn add  @craco/craco
```

2. 创建并配置 `craco.config.js` 文件

```js
const path = require('path')

module.exports = {
  // webpack 配置
  webpack: {
    // 配置别名
    alias: {
      // 约定：使用 @ 表示 src 文件所在路径
      '@': path.resolve(__dirname, 'src'),
      // 约定：使用 @scss 表示 样式 文件所在路径
      '@scss': path.resolve(__dirname, 'src', 'assets', 'styles')
    },
  },
}

```

3. `package.json `中修改命令

```json
// 将 start/build/test 三个命令修改为 craco 方式
"scripts": {
  "start": "craco start",
  "build": "craco build",
  "test": "craco test",
  "eject": "react-scripts eject"
}
```



## tsconfig.json

项目使用了 TS，而 TS 带有配置文件 tsconfig.json。因此，不需要再使用 jsconfig.json

VSCode 会自动读取 `tsconfig.json` 中的配置，让 vscode 知道 @ 就是 src 目录

创建并配置 `path.tsconfig.json`  文件

```
{
  "extends": {
      "compilerOptions": {
        "baseUrl": "./",
        "paths": {
          "@/*": ["src/*"]
        }
      }
   }
}
```



## 移动端适配

使用 `postcss-px-to-viewport` 包将 px 转换成 vw

**步骤如下**

1. 下包：使用 `postcss-px-to-viewport`包会出现版本不匹配错误，使用如下包解决

```
yarn add postcss-px-to-viewport-8-plugin
```

2. 在 `croco.config.js`文件中配置

```js
module.exports = {
// ... 省略其他配置
style: {
    postcss: {
      //   plugins: [vw]
			mode: 'extends',
			loaderOptions: {
				postcssOptions: {
					ident: 'postcss',
					plugins: [
						[
							'postcss-px-to-viewport-8-plugin',
							{
								viewportWidth: 375, // 设计稿的视口宽度
							},
						],
					],
				},
			},
		},
	},
}
```



## 移动端 1px 像素处理

css 的 1px 在 PC 端就是 1px， 在移动端就往往就 >1px 了，会使得线条过大

### 产生原因

- 设备像素比：dpr=window.devicePixelRatio，也就是设备的物理像素与逻辑像素的比值。
- 在`retina`屏的手机上, `dpr`为`2`或`3`，`css`里写的`1px`宽度映射到物理像素上就有`2px`或`3px`宽度。
- 例如：`iPhone6`的`dpr`为`2`，物理像素是`750`（x轴）,它的逻辑像素为`375`。也就是说，1个逻辑像素，在`x`轴和`y`轴方向，需要2个物理像素来显示，即：dpr=2时，表示1个CSS像素由4个物理像素点组成。

### **解决方式**

#### **1. 伪元素 + transform 缩放**

> 伪元素`::after`或`::before`独立于当前元素，可以单独对其缩放而不影响元素本身的缩放
>
> 参考：https://blog.csdn.net/xgangzai/article/details/107118168

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="author" content="helang.love@qq.com">
    <title>移动端 1px(线条/边框) 解决方案</title>
    <style type="text/css">
        body{
            margin: 0;
            padding: 0;
            font-size: 14px;
            color: #333;
            font-family: 'Microsoft YaHei', 'Times New Roman', Times, serif;
        }
 
        /* 线条 */
        .list{
            margin: 0 20px;
            list-style: none;
            line-height: 42px;
            padding: 0;
        }
        .list>li{
            padding: 0;
            position: relative;
        }
        .list>li:not(:first-child):after{   /* CSS匹配非第一个直接子元素 */
            content: "";
            display: block;
            height: 0;
            border-top: #999 solid 1px;
            width: 100%;
            position: absolute;
            top: 0;
            right: 0;
            transform: scaleY(0.5); /* 将 1px 的线条缩小为原来的 50% */
        }
 
        /* 边框 */
        .button{
            line-height: 42px;
            text-align: center;
            margin: 20px;
            background-color: #f8f8f8;
            position: relative;
            border-radius: 4px;
        }
        .button:after{
            content: "";
            position: absolute;
            top: -50%;
            right: -50%;
            bottom: -50%;
            left: -50%;
            border: 1px solid #999;
            transform: scale(0.5);
            transform-origin: 50% 50% 0;
            box-sizing: border-box;
            border-radius: 8px; /* 尺寸缩小 50%，即圆角半径设置为按钮的2倍 */
        }
    </style>
</head>
<body>
 <ul class="list">
    <li>线条 1px</li>
 </ul>
 <div class="button">边框 1px</div>
</body>
</html>
```



## sass 自定义函数

sass 中 @mixin 指令允许我们定义一个可以在整个样式表中重复使用的样式。

@include 指令可以将混入（mixin）引入到文档中。

[实现参考 antd-mobile](https://github.com/ant-design/ant-design-mobile/blob/v2/components/style/mixins/hairline.less)

- 定义解决 1px 函数创建公共样式

```
// src/assets/styles/hairline.scss

@mixin scale-hairline-common($color, $top, $right, $bottom, $left) {
  content: '';
  position: absolute;
  display: block;
  z-index: 1;
  top: $top;
  right: $right;
  bottom: $bottom;
  left: $left;
  background-color: $color;
}


@mixin hairline($direction, $color: #000, $radius: 0) {
  @if $direction == top {
    border-top: 1px solid $color;

    // min-resolution 用来检测设备的最小像素密度
    @media (min-resolution: 2dppx) {
      border-top: none;

      &::before {
        @include scale-hairline-common($color, 0, auto, auto, 0);
        width: 100%;
        height: 1px;
        transform-origin: 50% 50%;
        transform: scaleY(0.5);

        @media (min-resolution: 3dppx) {
          transform: scaleY(0.33);
        }
      }
    }
  } @else if $direction == right {
    border-right: 1px solid $color;

    @media (min-resolution: 2dppx) {
      border-right: none;

      &::after {
        @include scale-hairline-common($color, 0, 0, auto, auto);
        width: 1px;
        height: 100%;
        background: $color;
        transform-origin: 100% 50%;
        transform: scaleX(0.5);

        @media (min-resolution: 3dppx) {
          transform: scaleX(0.33);
        }
      }
    }
  } @else if $direction == bottom {
    border-bottom: 1px solid $color;

    @media (min-resolution: 2dppx) {
      border-bottom: none;

      &::after {
        @include scale-hairline-common($color, auto, auto, 0, 0);
        width: 100%;
        height: 1px;
        transform-origin: 50% 100%;
        transform: scaleY(0.5);

        @media (min-resolution: 3dppx) {
          transform: scaleY(0.33);
        }
      }
    }
  } @else if $direction == left {
    border-left: 1px solid $color;

    @media (min-resolution: 2dppx) {
      border-left: none;

      &::before {
        @include scale-hairline-common($color, 0, auto, auto, 0);
        width: 1px;
        height: 100%;
        transform-origin: 100% 50%;
        transform: scaleX(0.5);

        @media (min-resolution: 3dppx) {
          transform: scaleX(0.33);
        }
      }
    }
  } @else if $direction == all {
    border: 1px solid $color;
    border-radius: $radius;

    @media (min-resolution: 2dppx) {
      position: relative;
      border: none;

      &::before {
        content: '';
        position: absolute;
        left: 0;
        top: 0;
        width: 200%;
        height: 200%;
        border: 1px solid $color;
        border-radius: $radius * 2;
        transform-origin: 0 0;
        transform: scale(0.5);
        box-sizing: border-box;
        pointer-events: none;
      }
    }
  }
}

// 移除边框
@mixin hairline-remove($position: all) {
  @if $position == left {
    border-left: 0;
    &::before {
      display: none !important;
    }
  } @else if $position == right {
    border-right: 0;
    &::after {
      display: none !important;
    }
  } @else if $position == top {
    border-top: 0;
    &::before {
      display: none !important;
    }
  } @else if $position == bottom {
    border-bottom: 0;
    &::after {
      display: none !important;
    }
  } @else if $position == all {
    border: 0;
    &::before {
      display: none !important;
    }
    &::after {
      display: none !important;
    }
  }
}
```

- 在某个 scss 文件中使用

```scss
// 导入该文件后调用
@import '~@scss/hairline.scss';

.box1 {
  margin: 10px 0;
  position:relative;
  @include hairline(bottom, #000); // 添加边框
  @include hairline-remove(bottom); // 移除边框
}


```



## icon 彩色图标

[彩色图标文档](https://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.d8cf4382a&helptype=code)

**使用步骤**

1. 在 public 下 index.html 中引入生成的 js 链接
2. 在 `index.scss` 中添加通过 css 代码

```css
.icon {
  width: 1em; height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
```

3. 在组件中，使用

```js
<svg className="icon" aria-hidden="true">
  {/* 使用时，只需要将此处的 iconbtn_like_sel 替换为 icon 的名称即可*/}
  <use xlinkHref="#iconbtn_like_sel"></use>
</svg>


<svg className="icon" aria-hidden="true">
  {/* 使用时，只需要将此处的 iconbtn_like_sel 替换为 icon 的名称即可*/}
  <use xlinkHref="#xxxxxxxx"></use>
</svg>

```

项目中会封装 Icon 组件



## TS 类型文件

在使用 TS 开发与接口相关功能时，推荐按照以下步骤：

1. 准备数据类型。先按照接口的返回数据，准备 TS 类型
2. 指定该请求的返回值类型。在发送请求时，指定该请求的返回值类型

这样，在接下来的操作中，有两个好处：

1. 有类型提示。如果需要用到接口的数据，都会有类型提示了。
2. 有利于项目代码的重构和修改。如果将来后端接口返回的数据变了，只需要修改对应的 TS 类型，然后，所有用到该类型的地方如果有问题，都会自动提示出来。

在 types 目录下新建 `data.d.ts` 类型说明文件，新建 `store.d.ts` 集中放置 store 类型文件



## 路由鉴权

react 中没有路由守卫，但是类似的功能有现成的。[参考官网](https://v5.reactrouter.com/web/example/auth-workflow)

项目中定义私有路由组件来实现路由的访问登录控制 



## 401报错

有如下两种情况会出现401错误：

1. 未登陆用户做一些需要权限才能做的操作（例如：关注作者），代码会报出401错误。这种情况下，应该让用户回到登陆页。
2. 登录用户的 token 过期了

通常通过 axio s响应拦截器来处理401问题。



## websocket

WebSocket 是一种数据通信**协议**，类似于我们常见的 http 协议。

HTTP 协议单向请求的特点，注定了如果服务器有连续的状态变化，客户端要获知就非常麻烦。我们只能使用["轮询"](https://www.pubnub.com/blog/2014-12-01-http-long-polling/)：每隔一段时候，就发出一个询问，了解服务器有没有新的信息。最典型的场景就是聊天室。

轮询的效率低，非常浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）。因此，工程师们一直在思考，有没有更好的方法。WebSocket 就是这样发明的。

**简介**

该协议在2008年诞生，2011年成为国际标准。所有浏览器都已经支持了。

其最大特点就是，服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于[服务器推送技术](https://en.wikipedia.org/wiki/Push_technology)的一种。

典型的 websocket 应用场景：

+ 即时通讯.....客服  
+ 聊天室 广播
+ 点餐



### socket 技术

1. **客户端**：发 socket 请求
   1. 可以用原生的
   2. 可以使用包 **socket.io**
2. 服务器端: 提供 socket 服务
   1. **socket.io**

[测试案例](https://socketio-chat-h9jt.herokuapp.com/)



### socket.io

下包：`npm i socket.io-client`，该包专门在客户端使用

```tsx
import io from 'socket.io-client'

// 和服务器建立了链接
const client = io('地址', {
    query: {
        token: 用户token
    },
    transports: ['websocket']
})

// connect， disconnect 是固定的名字
client.on('connect', () => {})  // 当和服务器建立连接成功，这个事件就会触发
client.on('disconnect', () => {})    // 和服务器断开链接，就会触发disconnect


// message是事件名，这个可以由后端去修改
client.on('message', () => {})  // 接收到服务器的消息，这个事件就会触发
// 主动给服务器发送消息
client.emit('message', 值)


// 主动关闭和服务器的链接
client.close()
```

在 react 中借助于 `useEffect`，在进入页面时调用客户端库建立 socket.io 连接

```tsx
// 用于缓存 socket.io 客户端实例
const clientRef = useRef<Socket | null>(null)

useEffect(() => {
  // 创建客户端实例
  const client = io('http://toutiao.itheima.net', {
    transports: ['websocket'],
    // 在查询字符串参数中传递 token
    query: {
      token: getToken().token
    }
  })

  // 监听连接成功的事件
  client.on('connect', () => {
    // 向聊天记录中添加一条消息
    setMessageList(messageList => [
      ...messageList,
      { type: 'robot', text: '我现在恭候着您的提问。' }
    ])
  })

  // 监听收到消息的事件
  client.on('message', data => {
    console.log('>>>>收到 socket.io 消息:', data)
  })

  // 将客户端实例缓存到 ref 引用中
  clientRef.current = client

  // 在组件销毁时关闭 socket.io 的连接
  return () => {
    client.close()
  }
}, [])
```



## lodash

#### differenceBy() 

该方法可以用来过滤数据，其接收三个参数，分别是要检查的数组、排除的值、调用的每个元素，返回一个过滤值后的新数组。

[见中文文档](https://www.lodashjs.com/docs/lodash.differenceBy)

```tsx
import { differenceBy } from 'lodash'

const optionChannel = useSelector((state: RootState) => {
  // 推荐频道 = 所有频道 - 我的频道
  const { userChannel, allChannel } = state.home
  return differenceBy(allChannel, userChannel, 'id')
})
```

### debounce()

[见中文文档](https://www.lodashjs.com/docs/lodash.debounce)

该方法创建一个防抖函数，函数会从上一次被调用后，延迟 `wait` 毫秒后调用 `func` 方法

`_.debounce(func, [wait=0], [options=])`

```js
// 当点击时 `sendMail` 随后就被调用。
jQuery(element).on('click', _.debounce(sendMail, 300, {
  'leading': true,
  'trailing': false
}));
```





## dayjs

[相对时间处理](https://dayjs.gitee.io/docs/zh-CN/plugin/relative-time)

相对时间处理指将标准时间转换成相对时间。`2022-10-07` -> `1天前`

**步骤**

```js
import dayjs from 'dayjs'
import relativeTime from 'dayjs/plugin/relativeTime'
dayjs.extend(relativeTime)
// 国际化
import 'dayjs/locale/zh-cn'
dayjs.locale('zh-cn')

console.log( dayjs('2021-12-06').fromNow() )
```



## 图片懒加载

### 基本思路

当 dom元素进入可视区域时，才去加载它。

如何判断一个 dom 元素是否进入了可见区域？

- 传统： [获取dom的位置](https://gitee.com/link?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FElement%2FgetBoundingClientRect)，手动判断。（距离页面顶部的距离 比较 滚动条卷起的高度，还要考虑元素自己的高度 ）
- 现在：[直接判断元素进入可视区域的比例](https://gitee.com/link?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FIntersectionObserver%2FIntersectionObserver)

### 实现思路

利用浏览器提供的 [IntersectionObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver/IntersectionObserver)，监听图片元素是否进入可视区域，进入后才真正去设置图片元素的 `src` 属性进行图片加载。

```js
var dom = dom元素
// 实例化一个观察者
// 它的参数1是一个回调：当被观察的目标进入视口/离开视口就会调用
var observer = new IntersectionObserver((entries)=>{
    // 当观察者元素可见时 isIntersecting 为 true
  console.log(entries[0].isIntersecting)
  if(entries[0].isIntersecting) {
    // 可见时给图片 src 赋值
  }
}, 其他配置)

// 观察者观察 dom
observer.observe(dom)
observer.disconnect()   // 停止观察者
observer.unobserve(dom) // 观察者停止对 dom 的观察
// 可以观察多个 dom 元素，参见 https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver/observe
const boxElList = document.querySelectorAll('.box');
boxElList.forEach((el) => {
  observer.observe(el);
})
```

项目中可以封装懒加载的组件



##  classnames

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



## 防抖处理

在使用搜索功能时会需要用到防抖，可以自己写防抖，也可以使用第三方库

**实现**

```tsx
// 存储防抖定时器
const timerRef = useRef(-1)

const [keyword, setKeyword] = useState('')
const onChange = (e: string) => {
  setKeyword(e)
  // 清除之前的定时器
  clearTimeout(timerRef.current)
  // 新建任务定时器
  timerRef.current = window.setTimeout(() => {
    console.log(e)
  }, 500)
}
// 销毁组件时记得最好要清理定时器
useEffect(() => {
  return () => {
    clearTimeout(timerRef.current)
  }
}, [])
```

第三方库如 aHooks(useDebounceFn)、lodash(_.debounce)



## aHooks

> ahooks，是一套高质量可靠的 React Hooks 库。在 React 项目研发过程中，一套好用的 React Hooks 库是必不可少的。

### useDebounceFn

用来处理防抖函数的 Hook。[官网文档](https://ahooks-next.surge.sh/zh-CN/hooks/use-debounce-fn)

频繁调用 run，但只会在所有点击完成 500ms 后执行一次相关函数

```tsx
import { useDebounceFn } from 'ahooks'

const { run } = useDebounceFn(
	(e) => {
		console.log('搜索功能', e)
	},
	{ wait: 500 }
)
```





## 高亮关键字处理

![img](C:/Users/矿机/Desktop/Blog/source/img/React 移动端总结/image-20211206185240350.png)

一般而言封装一个高亮函数。在目标字符串中，用正则做匹配，如果匹配到，就用参数2的返回值来替代匹配的部分。[官方文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

```js
// const 新字符串 = 目标字符串.replace(正则表达式，function(匹配结果) {
//             return 对匹配到的内容的替换结果
// })

const highLight = (str: string， keyword: string) => {
  return str.replace(
    new RegExp(keyword, 'gi'),
    (match) => `<span>${match}</span>`
  )
}
```



## innerHTML

在 react 中，官方推出了 `dangerouslySetInnerHTML` 属性，[详见官网](https://zh-hans.reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml)

```tsx
<div>{'<h1>abc</h1>'}</div>
<div dangerouslySetInnerHTML={{ __html: '<h1>abc</h1>' }} />
```



## 防 XSS 攻击

当渲染 code 标签时，可以通过 `highlight.js` 包来实现代码块高亮。例子如下

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

但是该包不能避免 XSS 攻击。使用 `dompurify` 可对 HTML 内容进行净化处理

```tsx
// 下包：	yarn add dompurify @types/dompurify -D

import DOMPurify from 'dompurify'

<div
  className="content-html dg-html"
  dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(info.content || '') }}
  >
</div>
```



## keep-alive 功能

react 中路由没有自带缓存功能，每次路由切换页面组件都会销毁。

通过封装自定义路由来实现缓存效果。

**原理**：切换路由时让页面不卸载，而是通过 display none 隐藏掉。因为页面没有卸载，所以原来所有的操作都会被保存下来。 将来再返回该页面，只需要 display block 展示即可。这样，就可以恢复原来的内容达到缓存的目的。

### 背景知识

1. Switch 之外的 Route 中如果设置了 children 属性，且值是函数，此时，这个函数一定会执行；

```jsx
<Route path="/any-path" children={()=><div>child</div>}></Route>
```

2. Switch 之内的 Route 中如果设置了 children 属性，且值是函数，此时，它的 path 匹配优先级是最低的。

```tsx
/* eslint-disable react/no-children-prop */
import React from 'react'
import { Route, RouteChildrenProps, RouteProps } from 'react-router'
type Props = RouteProps
export default function KeepAlive ({ path, children, ...rest }: Props) {
  const child = (props: RouteChildrenProps) => {
    console.log('child....', props)
    // const isMatch = props.location.pathname.startsWith(path as string)
    return (
      <div
        className="keep-alive"
        style={{ display: props.match ? 'block' : 'none' }}>
        {children}
      </div>
    )
  }

  return <Route path={path} {...rest} children={child} />
}

```

需要缓存的页面直接使用即可

```tsx
<KeepAlive path="/home">
    <Home />
</KeepAlive>
```



## Diff 算法说明

- 如果两棵树的**根元素类型**不同，React 会销毁旧树，创建新树

```js
// 旧树
<div>
  <Counter />
</div>

// 新树
<span>
  <Counter />
</span>

执行过程：destory Counter -> insert Counter
```

- 对于类型相同的 React DOM 元素，React 会对比两者的属性是否相同，只更新不同的属性
- 当处理完这个 DOM 节点，React 就会递归处理子节点

```js
// 旧
<div className="before" title="stuff"></div>
// 新
<div className="after" title="stuff"></div>
只更新：className 属性

// 旧
<div style={{color: 'red', fontWeight: 'bold'}}></div>
// 新
<div style={{color: 'green', fontWeight: 'bold'}}></div>
只更新：color属性
```

- 当在子节点的后面添加一个节点，这时候转化工作执行的很好

```js
// 旧
<ul>
  <li>first</li>
  <li>second</li>
</ul>

// 新
<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>

执行过程：
React会匹配新旧两个<li>first</li>，匹配两个<li>second</li>，然后添加 <li>third</li> tree
```

但是如果在开始位置插入一个元素，那么问题就来了：

```js
// 旧
<ul>
  <li>1</li>
  <li>2</li>
</ul>

// 新
<ul>
  <li>3</li>
  <li>1</li>
  <li>2</li>
</ul>

执行过程：
React将改变每一个子节点，而非保持 <li>Duke</li> 和 <li>Villanova</li> 不变
```

### key 属性

为了解决以上问题，React 提供了一个 key 属性。当子节点带有 key 属性，React 会通过 key 来匹配原始树和后来的树。

```js
// 旧
<ul>
  <li key="2015">1</li>
  <li key="2016">2</li>
</ul>

// 新
<ul>
  <li key="2014">3</li>
  <li key="2015">1</li>
  <li key="2016">2</li>
</ul>

执行过程：
现在 React 知道带有key '2014' 的元素是新的，对于 '2015' 和 '2016' 仅仅移动位置即可
```

- 说明：key 属性在 React 内部使用，但不会传递给你的组件
- 推荐：在遍历数据时，推荐在组件中使用 key 属性：`<li key={item.id}>{item.name}</li>`
- 注意：**key 只需要保持与他的兄弟节点唯一即可，不需要全局唯一**
- 注意：**尽可能的减少数组 index 作为 key，数组中插入元素的等操作时，会使得效率低下**