---
title: WEB API 总结
tags: WEB API
abbrlink: ea4ca316
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/webapi.webp
---



# WEB API 总结

### 1. DOM 概念

指文档对象模型。 文档对象模型（Document Object Model，简称DOM）,是浏览器提供的一套专门用来 动态操作网页内容 的API(函数) , 其核心就是把内容当对象来处理  ,网页所有内容都在document里面

### 2. 获取 DOM 对象的方法

- document.querySelector('选择器'): 根据CSS选择器来获取DOM元素 会返回 dom 对象
- document.querySelectorAll('选择器'): 会返回伪数组, 包含所有的 dom 对象
- document.getElementById('选择器'): 根据 id 选择器来获取 dom 元素
- document.getElementsByClassName("类名"): 根据类名获取 dom,也是返回伪数组

###### 设置/修改DOM元素内容 : 

- 对象. innerText: 获取 dom 元素的文本信息,无法解析标签
- 对象. innerHTML: 获取 dom 元素的所有信息, 包括标签内容

###### 设置/修改标签元素属性: 

- 对象.href : 可以修改 dom 对象的 a 标签链接
- 对象.src : 可以修改 img 标签的路径
- 对象.value : 获取/修改表单值
- 对象.disabled :  设置是否禁用表单元素        disabled = true(禁用)
- 对象.checked:  设置是否选中(radio  checkbox)
- 对象.selected: 设置是否选中(下拉菜单option标签)

###### 设置/修改元素样式属性

- 对象.style.样式 = '': 用来修改 dom 元素的样式, 属性有-连接符，要转换为小驼峰命名法

###### 操作类名

- 对象.className = 类名 : 可以修改 dom 元素类名, 修改后会覆盖原有的类名
- 对象. classList:  不会覆盖原有的类名 .add 添加类名  .remove 移除类名  .toggle 切换类名

### 3. 事件

​	事件指需要通过鼠标或键盘去触发的操作, 是js中用于实现交互功能的一种技术

​	包含三要素: 事件源/事件类型/事件处理函数, 有两种语法

```
// 点语法注册事件 事件源.事件类型 = 事件处理函数 该语法不支持注册多同名事件
// 事件源.addEvenetListener('事件类型',事件处理函数) 可用于注册多个同名事件,依次触发
//  事件源.removeEventListener('事件类型',事件处理函数) 用于移除事件
//  阻止默认事件行为 ：  e.preventDefault()
// 事件对象: e.target 存储与事件触发相关的数据,触发事件时，浏览器会捕捉事件触发相关的数据（鼠标坐标点、键盘按键），存入一个对象中，称之为事件对象
// 获取键盘按键 e.key  : 获取具体按键字符串   'Enter'
//  e.keyCode : 获取键盘ASCII码	e.pageX/e.pageY : 获取鼠标触发点位置
```

​	事件类型包括鼠标事件和键盘事件

- onclick(): 鼠标单击事件
- ondblclick: 鼠标双击
- oninput : 键盘输入
- onmouseenter/onmouseover : 鼠标移入
- onmouseleave/onmouseout : 鼠标移出
- oninput: 获取光标
- onblur: 失去焦点
- onfocus: 成为焦点
- onKeydown: 按下键盘
- onKeyup: 松开键盘
- onscroll: 滚动事件
- onresize:  窗口( 页面大小 )变化事件

### 4.  自定义属性 Attribute

属性分为标准属性和自定义属性, 标准属性是 html 和 css 所有的属性, 浏览器可以识别

浏览器不能识别样式的属性，用户自定义添加（用于存储一些数据)属于自定义属性

```js
 元素.setAttribute('属性名',属性值) // 自定义添加属性
 元素.getAttribute('属性名') // 获取属性
 元素.removeAttribute('属性名') // 移除属性
 
 // 设置某元素 css 属性时可以用
document.querySelector('main').setAttribute('style', 'display:none !important')
```

### 5. DOM 树节点操作

​	DOM树里每一个内容都称之为节点, 有元素节点(所有的标签)   属性节点   注释节点    文本节点

- 获取子节点与子元素

```js
// 父元素.childNodes( 获得所有子节点, 包括文本节点（空格、换行）、注释节点等)
// 获取子元素节点（重点）:   父元素.children
```

- 获取兄弟元素

```js
// 获取上一个兄弟元素节点 : 元素.previousElementSibling
// 获取下一个兄弟元素节点 : 元素.nextElementSibling
```

-  获取父元素节点  

```js
// 子元素.parentNode	返回最近一级的父节点 找不到返回 null
//  父元素 => body => html => document => null 
```

- 新增节点

```js
// 1.创建空标签 ;    let li = document.createElement('标签名')  
// 2.设置节点内容;	 li.innerText = '我是新增加的li标签'
// 3.追加到页面; 
//	(1).追加到最后面  父元素.appendChild(子元素)	
//	(2) .追加到指定位置  父元素.insertBefore(子元素,要加到哪个元素的前面)
```

- 克隆节点

```js
//    元素.cloneNode(布尔类型值)  默认false：不克隆后代节点元素
//	若为true，则代表克隆时会包含所有后代节点元素一起克隆
//	若为false，则代表克隆时不包含后代节点
```

-  删除节点

```js
//	父元素.removeChild(子元素) 删除父元素的子元素
// 删除和隐藏节点（display:none）的区别：隐藏节点还是存在，但是删除则从 html 中删除节点
```

### 6. 重绘和回流(重排)

- 重绘:  由于节点(元素)的样式改变不会影响其在文档流中的位置和布局(比如：color、background-color、outline等)
- 回流(重排): 当 Render Tree 中部分或者全部元素的尺寸、结构、布局等发生改变时，浏览器就会重新渲染部分或全部文档的过程称为 回流。
- 重绘不一定引起回流，而回流一定会引起重绘。

### 7. 定时器

- 永久定时器：一旦开启，永久重复执行，只能手动清除

```js
// 开启定时器:  setInterval(一段代码,间隔时间),会返回定时器 ID
// 清除定时器:  clearInterval(定时器ID)
```

- 一次定时器：开启之后间隔时间只会执行一次， 执行完成后自动清除

```js
// 开启定时器:  setTimeout(一段代码,间隔时间)
// 清除定时器:  clearInterval(定时器ID)
```

### 8. 事件冒泡和捕获事件

事件冒泡: 当触发子元素的事件时，该元素的所有“父级元素” 的“同名事件”会依次触发 

事件委托: 给父元素注册事件,委托子元素来处理,其中 e.target 代表被触发的子元素

事件捕获 ：当子元素事件被触发的时候，会先从最顶级的父元素一级一级往下触发

注册事件捕获, 只能通过 addEventListener('事件类型',事件处理函数,true)来触发,点语法无法注册,

第三个参数默认值是false(冒泡)  true(捕获)

阻止事件流动 : 阻止冒泡 + 捕获:  e.stopPropagation()

### 9. 三大家族

1. offset : 获取元素自身的真实宽高与位置, 获取的都为数字

```js
// offsetWidth/offsetHeight :  真实宽高 = content + padding*2 + border*2
// offsetLeft/offsetTop : 自身左/上外边框 到定位父元素左/上内边框距离
```

2. scroll : 获取元素内容的真实宽高与位置

```js
//  scrollWidth/scrollHeight :  内容真实宽高
//  scrollLeft/scrollTop : 滚动条滚动左/上边的距离 (该值可以修改)
```

3. client:  获取元素可视区域的宽高与位置

```js
//  clientWidth/clientHeight :  获取可视区域宽高    
//	clientLeft/clientTop : 获取可视区域位置 （其实就是左边框 与 上边框宽度）
```

### 10. 响应式布局

- 媒体查询
- 窗口监测事件:

```js
 window.onresize = function(){
	 document.documentElement.clientWidth,
     document.documentElement.clientHeight // 窗口宽高
}
```

### 11. window 对象

window 对象是 js 中顶级对象,所有的全局变量都是其对象的属性

- window.open() : 打开新窗口
- window.close() : 关闭当前窗口
- window.onload:界面上所有的内容加载完毕之后才触发这个事件
- window.onbeforeunload:界面在关闭之前会触发这个事件
- window.onunload:界面在关闭的那一瞬间会触发这个事件

location 对象: 包含当前页面的URL信息

- location.hash: 资源定位符（锚点定位）
- location.host :  主机 host = hostname + port
- location.port: 端口号
- location.href: 完整的url路径
- location.search:  url 请求的参数
- location.assign('http:// www.baidu.com'): 打开新网页，会留下历史记录（可以回退）
- location.replace():  打开新网页，不会留下历史记录（不能回退）
- location.reload() : 刷新当前网页,相当于按了F5刷新当前网页

history对象: 主要用于记录你当前窗口的历史记录,主要作用就是前进和后退网页

- history.forword():前进
- history.back():后退

navigator对象: 包含当前浏览器的信息

- navigator.appVersion 当前浏览器版本信息
- navigator.userAgent 当前浏览器信息

screen对象: 获取电脑屏幕像素

- screen.width/height: 获取屏幕像素

### 12 . localstorage与sessionstorage

适用于数据存储, localStorage 将数据存在硬盘中, sessionstorage 存在内存中,二者用法完全一样

```js
localStorage/sessionstorage.setItem('属性名','属性值') // 存
localStorage/sessionstorage.getItem('属性名') // 取
localStorage/sessionstorage.removeItem('属性名') // 删
localStorage/sessionstorage.clear() // 清空
```

### 13. JSON 数据类型

json 是一种字符串格式, 为了解决跨平台兼容性

```js
// json转js : JSON.parse(json对象)
// js转json :  JSON.stringify(js对象)
```

### 14. 正则表达式

正则表达式是一个对字符串实现逻辑匹配运算的对象,可以按照某种某种规则匹配字符串

- test() 方法匹配规则, 满足返回 true
- replace(规则,替换内容) : 根据规则替换内容

两种方法使用正则表达式: 1.	创建正则表达式对象	`new RegExp()`

2. 使用字面量  `/ 规则 /`

- 元字符: 就是字符本身的含义,比如说 /abc/ 就是检测字符串中是否有 abc 
- 原义文本字符 : 类似于 js 关键字,改变了本身的含义, `. \ | [] {} () + ? * $ ^`
- 字符类: 指符合某些特征的对象,  只是泛指 ` /[abc]/` 匹配字符串中只要有 a/b/c任意即可
- 范围类: 指定义范围内的任意字符,`[a-zA-Z0-9]`指字母 a-z 中任意字母均可
- 预定义类: 指已经预先定义好的有特殊含义的字符

```js
  /* 预定义类    等价类             含义
             .          [^\r\n]           除了回车和换行之外的所有字符
           \d           [0-9]             数字字符
           \D           [^0-9]            非数字字符
           \s           [\f\n\r\t\v]      空白字符
           \S           [^\f\n\r\t\v]     非空白字符
           \w           [a-zA-Z_0-9]      单词字符（字母、下划线、数字）
           \W           [^a-zA-Z_0-9]     非单词字符 */
```

- 边界: 指某一个位置, 如 `^` 表示以 什么开头 , `$` 表示以什么结尾
- 量词: 表示允许字符出现的数量

```js
 /*  量词             含义
            ?                出现零次或一次（最多出现一次）
            +                出现一次或多次（至少出现一次）
            *                出现零次或多次（任意次）
            {n}              出现n次
            {n,m}            出现n-m次
            {n,}             出现至少n次（>=n） */
```

- 分组: 指将某一类字符归为一组, `()`  和  `|` 
- 修饰符: 指预先定义的影响整个规则的特殊符号,写在末尾,可以连写

```js
 /*修饰符：影响整个正则规则的特殊符号
        书写位置：  /pattern/modifiers(修饰符)
        i (intensity)：大小写不敏感（不区分大小写）
        g (global) : 全局匹配
        m(multiple) : 检测换行符，使用较少，主要影响字符串的开始^与结束$边界
  */
```



