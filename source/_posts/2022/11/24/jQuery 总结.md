---
title: jQuery 总结
tags: jQuery
abbrlink: ff587756
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/jQuery.webp
---

# jQuery 总结

   jQuery是一个快速、小巧且功能丰富的JavaScript库, 使HTML文档遍历和操作、事件处理、动画和Ajax等操作变得更加简单,  并具有易于使用的API

##### 入口函数

 $(document).ready( function(){} )  可以简写成  $( function(){} )

等待 dom 树加载完毕后执行,且可以执行多次,  而原生的要等到 dom 树 + 外部资源路径加载完后执行,且原生的只能执行一次

##### 原生 dom 与 jq 区别

dom 对象: 使用 dom 原生语法获取的对象

jq 对象: 只能使用 jq 语法, 无法使用原生 dom 语法

dom => jq	: 	$( dom 对象 )		jq => dom	: 	$(选择器)[下标] /  $(选择器).get(下标)

```js
$(document.querySlector('div'))		$('div')[0]	|| $('div').get(2)
```

##### $ 函数三种功能

-  如果参数是一个函数 ： 入口函数   $(function(){})
- 如果参数是一个选择器 ： 查询jq对象  $('#box')
- 如果参数是一个DOM对象 ： DOM->jq

#####  选择器

- 基本选择器

| 名称       | 用法               | 描述                                 |
| ---------- | ------------------ | :----------------------------------- |
| ID选择器   | $('#id');          | 获取指定ID的元素                     |
| 类选择器   | $('.class');       | 获取同一类class的元素                |
| 标签选择器 | $('div');          | 获取同一类标签的所有元素             |
| 并集选择器 | $('div,p,li');     | 使用逗号分隔，只要符合条件之一就可。 |
| 交集选择器 | $('div.redClass'); | 获取class为redClass的div元素         |

- 层级选择器

| 称         | 用法          | 描述                                                        |
| ---------- | ------------- | :---------------------------------------------------------- |
| 子代选择器 | $('ul > li'); | 使用>号，获取儿子层级的元素，注意，并不会获取孙子层级的元素 |
| 后代选择器 | $('ul li');   | 使用空格，代表后代选择器，获取ul下的所有li元素，包括孙子等  |

- 过滤选择器: 这类选择器都带冒号

| 名称         | 用法                               | 描述                                                        |
| ------------ | ---------------------------------- | :---------------------------------------------------------- |
| :eq（index） | $('li:eq(2)').css('color', 'red'); | 获取到的li元素中，选择索引号为2的元素，索引号index从0开始。 |
| :odd         | $('li:odd').css('color', 'red');   | 获取到的li元素中，选择索引号为奇数的元素                    |
| :even        | $('li:even').css('color', 'red');  | 获取到的li元素中，选择索引号为偶数的元素                    |

##### css 属性操作

jq语法操作css样式   =>  调用方法

```js
//  查询css样式 ： $().css('属性名')
//  设置css样式 :  $().css('属性名',属性值)
//  设置多个样式: $().css( { '属性名':属性值, '属性名',属性值} )
```

##### 注册事件

jq注册事件本质 ： 调用函数		$().click(事件处理函数);

```js
//	$(选择器).事件类型(事件处理函数)	// 不支持事件委托 且有少部分事件不支持，例如 oninput
//  $('元素').on('事件类型',事件处理函数)	// 不需要委托 更推荐这种方式
//	$('父元素').on('事件类型','委托子元素',事件处理函数) // 需要委托
```

##### 属性操作

获取文本:	$().text()	获取标签内容:	 $().html()

获取/设置标准属性+自定义属性:	$().attr( 属性名)

移除标准属性+自定义属性::	$().removeAttr( 属性名 )

```js
//	$('#box').text()	// 获取文本
//	$('#box').html()	// 获取标签内容
//  $('a').attr('href','http://www.baidu.com')	// 设置 a 标签的 href 属性
//	 $('img').attr('src','./images/0002.jpg')	// 设置 img 的 src 属性
```

##### 表单元素属性操作

获取/设置文本： $( 表单元素 ).val( )

获取/设置表单元素特有的 checked / disabled / selected 属性:	$().prop( 属性名, 属性值)

```js
//	$('input:eq(0)').val()	// 获取第一个 input 元素的 value 文本
// 去除两边空格
$('input:eq(0)').val().trim()
//	$('input:eq(0)').val('今天你好')	// 设置第一个 input 元素的 value 文本
//	$('input:eq(0)').prop('disabled')	// 获取第一个 input disabled 属性值
//	$('input:eq(0)').prop('disabled', true)	// 设置第一个 input disabled 属性值 为 true
//  $('select').val( '选中option的value值' )	// 设置select下拉菜单的选中元素
//	$('选中的option').prop('selected',true)	// 设置select下拉菜单的选中元素
// 重置表单内容	 $('form') 默认是个数组
 $('form')[0].reset()
```

##### 节点操作

| 名称               | 用法                        | 描述                                   |
| ------------------ | --------------------------- | :------------------------------------- |
| children(selector) | $('ul').children('li')      | 相当于$('ul>li'),子类选择器,获取子元素 |
| find(selector)     | $('ul').find('li');         | 相当于$('ul li'),后代选择器            |
| siblings(selector) | $('#first').siblings('li'); | 查找兄弟节点，不包括自己本身。         |
| parent()           | $('#first').parent();       | 查找父元素,不支持筛选                  |
| eq(index)          | $('li').eq(2);              | 相当于$('li:eq(2)'),index从0开始       |
| next()             | $('li').next()              | 找下一个兄弟元素                       |
| prev()             | $('li').prev()              | 找上一个兄弟元素                       |
| parents            | $('li').parents('ul')       | 查找所有父元素中符合条件的父元素       |

##### 创建元素

- 类似于 dom 中的创建方式 : 	$().html( '标签字符串' ) 
- 底层原理就是  document.createElement():   $('标签字符串')   

##### 添加元素

|    名称    | 用法                    | 描述                                 |
| :--------: | ----------------------- | ------------------------------------ |
|  append()  | 父元素.append(子元素)   | 添加到父元素最后面                   |
| appendTo() | 子元素.appendTo(父元素) | 添加到父元素最后面(作用与append一致) |
| prepend()  | 父元素.prepend(子元素)  | 添加到最前面                         |
|  before()  | 兄弟A.before(兄弟B)     | B 插入A 前面                         |
|  after()   | 兄弟A.after(兄弟B)      | B 插入A 后面                         |

##### 移除元素

| 名称         | 用法            | 描述                 |
| ------------ | --------------- | -------------------- |
| $().html('') | 父元素.html('') | 移除所有子节点       |
| $().empty()  | 父元素.empty()  | 移除所有子节点       |
| $().remove() | 元素.remove()   | 移除自己/ 单个子节点 |

##### 显示与隐藏

- 显示: $().show()    底层是设置display为block
-  隐藏： $().hide()    底层是设置display为none

##### 类名操作

| 名称          | 用法                        | 描述                         |
| ------------- | --------------------------- | ---------------------------- |
| addClass()    | $('div').addClass('one')    | 添加类名                     |
| removeClass() | $('div').removeClass('one') | 移除类名                     |
| hasClass()    | $('div').hasClass('one')    | 判断类名, 返回布尔值         |
| toggleClass() | $('div').toggleClass('one') | 切换类名(有就移除，没就添加) |

##### 链式编程

链式编程 ： 一个对象可以连续调用它的方法

链式编程原理 ： 在对象的方法中返回对象自身

当调用某个对象的方法时, 方法本身 返回这个对象, 这样就可以继续用点语法调用方法了,这就是链式编程原理.

#####  index 方法

index() 方法返回指定元素相对于其他指定元素的 index 位置, 如果没找到返回 -1

```js
$("li").click(function(){
  alert($(this).index()) // 获取点击的 li 元素下标,如果未找到返回 -1
})
```

##### 动画

| 名称        | 用法                                                  | 描述                                                         |
| ----------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| show()      | $('div').show(时间,速度,完成回调)                     | 展示动画，主要修改元素 `宽高 + display为block`               |
| hide()      | $('div').hide(时间,速度,完成回调)                     | 隐藏动画，主要修改元素 `宽高 + display为none`                |
| slideDown() | $('div').slideDown(动画时间，完成回调)                | 滑入动画（卷帘门效果），主要修改元素 `高度+ display为block`  |
| slideUp()   | $('div').slideUp(动画时间，动画完成回调)              | 滑出动画（卷帘门效果），主要修改元素 `高度+ display为none`   |
| fadeIn()    | $('div').fadeIn(动画时间,动画完成回调)                | 淡入动画(渐变效果)，主要修改元素 `透明度1+ display为block`   |
| fadeOut()   | $('div').fadeOut(动画时间,动画完成回调)               | 淡出动画(渐变效果)，主要修改元素 `透明度0+ display为block`   |
| fadeTo()    | $('div').fadeTo(动画时间，`目标透明度`,动画完成回调)  | 淡入动画(渐变效果)，主要修改元素 `透明度+ display为block`    |
| animate()   | $('div').animate(属性对象,动画时间,动画速度,回调函数) | 相当于webApi中封装的缓动动画animationSlow()，只是参数不同而已 |
| stop()      | $('div').stop(true,false)                             | 两个布尔值, 第一个为 true 表示立即结束动画,第二个表示是否跳转本次动画最终位置 |

```js
//(selector).hide/show/toggle(speed,linear/swing,callback)	//隐藏/显示/切换动画
// 均为可选参数,可以取值"slow"、"fast" 或毫秒。jq 自身提供 linear/swing ,以及回调函数

//$(selector).slideDown/slideUp/slideToggle(speed,callback) //向下/向上/切换元素
// 参数等同上, 没有动画时间, 可以用毫秒表示速度

// $(selector).fadeIn/fadeOut/fadeToggle/fadeTo(speed,callback) 
//  淡入/淡出/切换/淡入到指定透明度 , 参数同上 ,其中 fadeTo 透明度从 0-1 

// $(selector).stop(stopAll,goToEnd) // 停止动画
// 第一个表示是否清除动画队列	第二个表示是否跳转本次动画最终位置

// $(selector).animate({params},speed,callback) // 创建自定义动画
// 参数分别是 动画属性对象/动画时间/速度/回调函数
```

##### jq 三大家族

| 名称                               | 用法                      | 描述                                                         |
| ---------------------------------- | ------------------------- | ------------------------------------------------------------ |
| outerWidth() / outerHieght()       | $('div').outerWidth()     | 获取 `width` + `padding·`+ `border`(相当于原生的offsetWidth/Height) |
| width() / height()                 | $('div').width()          | 获取 `width`                                                 |
| innerWidth()/innerHeight()         | $('div').innerWidth()     | 获取 `wdith` + `padding`                                     |
| outerWidth(true)/outerHeight(true) | $('div').outerWidth(true) | 获取 `width` + `padding·`+ `border` + `margin`               |
| position()                         | $('div').position()       | 对象类型，自身左外边框  到 定位父级 左内边框距离（相当于原生的offsetLeft/Top） |
| offset()                           | $('div').offset()         | 对象类型, 自身左外边框 距离 页面 左内边框距离                |
| scrollLeft() / scrollTop()         | $('div').scrollLeft()     | 与原生的scrollLeft/Top作用一样.支持修改                      |

```js
$().width/height() 				// 获取 width/height
$().innerWidth/innerHeight()	// 获取 width + padding
$().outerWidth/outerHeight()    // 获取 width + padding + border
$().outerWidth/outerHeight(true)// 获取 width + padding + border+ margin
$().position()	//  获取 自身左/上 外边框 到定位父元素 左/上 内边框距离 获取的值不能修改
$().offset()  	//  获取 自身左/上 到 页面左/上 的距离, 获取的对象可以修改
$(window).width()	// 取页面可视区域大小 ，jq固定语法, 类似于 client 家族
$().scrollTop/scrollLeft()  // 获取滚动条位置 , 本质都是对象方法
```

