---
title: Vue3 总结
tags: Vue3
abbrlink: 40f5b380
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/vue3.png
---



# Vue3 总结

### 1. 基本概念

​	2022年 vue3 成为了 vue 默认版本,会不断有企业投入使用,相对于 vue2 版本,整体做了很多优化

- 性能大幅提升, 首次渲染更快, diff 算法更快, 内存占用更小,打包体积更小
- 更好的支持 typescript ,这也是未来的趋势
- vue2 是选项式 api , data 中写数据, methods 中写函数,一个功能代码分散,而 vue3 中兼容了选项式写法,同时全面拥抱 `Compositon API` ,即组合式 api , 一个功能逻辑代码写在一起,便于阅读和维护

### 2.vite 的使用

vite 是一个更加轻量级的 vue 项目打包工具, 热更新速度快,打包构建快,但是其默认安装插件非常少,需要额外配置. 了解即可,还是以 vue-cli 为主

- 创建项目 `npm init vite-app 项目名称` 或者 `yarn create vite-app 项目名称`
- 安装依赖 `npm i` 或者 `yarn`
- 启动项目 `npm run dev` 或者 `yarn dev`

### 3.生命周期函数

组合式 api 使用 生命周期函数,需先按需导入, 在 setup 函数中调用并传入回调函数,且可以多次调用

```js
import { onMounted } from 'vue'	// 先导入
  setup() {
  // 可以多次执行, 调用传入回调函数
    onMounted(() => {
      console.log('mouted生命周期执行了')
    })
    onMounted(() => {
      console.log('mouted生命周期函数又执行了')
    })
  }
```



| 选项式 API      | 组合式 API/Vue3               |
| --------------- | ----------------------------- |
| `beforeCreate`  | 不需要                        |
| `created`       | 不需要（直接写到setup函数中） |
| `beforeMount`   | `onBeforeMount`               |
| `mounted`       | `onMounted`                   |
| beforeUpdate    | `onBeforeUpdate`              |
| updated         | `onUpdated`                   |
| `beforeDestroy` | `onBeforeUnmount`             |
| `destroyed`     | `onUnmounted`                 |

### 4. setup 入口函数

setup 函数是新的组件选项,组合式 API 起点,只会在组件初始化时执行一次,没有 this. 其本质就是一个自执行函数, 返回一个对象,通常用来定义响应式数据

### 5. 响应式 api 

- ref 函数: 用于定义响应式数据类型(简单类型), 先导入,然后调用并传入数据,最后在 setup 中将返回值返回出去,函数中访问 ref 数据要通过 .value 访问,模板中不需要,代替了 vue2 中的 this

```js
import { ref } from 'vue' // 导入函数
setup() {
	// 定义数据
    let money = ref(100)
    // 访问数据通过 .value
    console.log(money.value)
    return { money }
  }
```

- reactive 函数: 用于定义复杂类型的响应式数据,先导入,然后调用传入数据,最后 setup 中返回出去,通常用来定义响应式  **对象数据**, 不支持定义简单类型的数据

```js
import { reactive } from 'vue' // 导入函数
setup () {
	// 定义响应式对象,调用函数传入对象
    const state = reactive({
      name: 'zs',
      age: 18
    })
    return { state }
// 定义简单数据类型用 ref , 定义对象用 reactive
```

- toRef 函数: 将响应式对象中某个属性变为单独响应式数据,并且值是关联的

```js
import { reactive, toRef } from 'vue' 	// 导入函数
 setup () {
    // 1. 定义响应式数据对象
    const obj = reactive({
      name: 'ls',
      age: 10
    })
    // 如果直接解构,数据会变成不是响应式的
    // let { name } = obj
    // 通过 toRef 函数将某个属性转换为单个响应式数据, 通过 .value 访问数据
   	 const name = toRef(obj, 'name')
     return {name}
  } 
// 常用于有一个响应式对象,但是模板中只需要使用其中一项数据
```

- toRefs 函数: 将响应式数据中所有属性定义为响应式数据, 通常用于解构或展开 reactive 定义的对象

```js
import { reactive, toRef, toRefs } from 'vue' // 导入对象
 setup () {
    // 1. 响应式数据对象
    const obj = reactive({
      name: 'ls',
      age: 10
    })
    // 如果直接解构,数据会变成不是响应式的
    // let { name } = obj
    // 通过 toRefs 函数转换, 所有属性都变成响应式数据
     const obj3 = toRefs(obj)
     return {...obj3, updateName}
 }
```

- computed 函数: 根据现有的响应式数据经过计算得到全新的响应式数据, 跟选项式 api 中的计算属性差不多. 导入=> 使用=> 定义计算方法 => return 返回值

```js
import { computed, ref } from 'vue'	// 导入函数
 setup() {
 	// 定义响应式数据
    const list = ref([1, 2, 3, 4, 5, 6])
    // 使用函数, 传入计算方法的函数, return 计算结果, 该 filterList 就是一个响应式的数		据,依赖于 list 值的变化而变化
    const filterList = computed(() => {
      return list.value.filter(item => item > 2)
    })
     return {list, filterList }
    }
    
```

- watch 函数:  类似于选项式 api 中的侦听属性,侦听响应式数据的变化而执行对应函数

```js
import { ref, watch } from 'vue' // 导入函数
 setup() {
     // 定义响应式数据
    const age = ref(18)
    // 调用函数传入参数进行侦听,第一个参数为要侦听的响应式数据,可以直接写要侦听的对象,或者是一个函数,返回要侦听的属性,尽量详细表明要侦听的属性,避免使用 deep 引起性能问题
    // 第二个参数为数据变化执行的逻辑代码
    // 第三个参数为配置对象,可以配置 immediate 和 deep 属性
    watch(() => {
      return age.value
    },  (newValue, oldValue) => {
      // 数据变化之后的回调函数
      console.log('age发生了变化')
    },{ immediate: true, deep: true})
    return { age }
  }
```

### 6. 父子通讯

组合式 API 中,父传子通过 props 接收,子传父通过自定义事件完成,在 setup 函数中有两个参数,第一个参数为 props, 包含了父组件传递过来的所有数据 , 第二个参数为 context, 包含了 emit 属性,可以触发自定义事件完成子传父

```js
<son :msg="msg" @get-msg="getMsg"></son>
// 定义响应式数据 传递给 子组件
 const msg = ref('this is msg')
    function getMsg(msg) {
      console.log(msg)
    }
    return { msg, getMsg }
// 子组件通过 props 接收
 props: ['msg'],
 // props 接收所有父组件传过来的数据, context 包含 emit 属性,触发自定义事件,均可解构
 setup(props,context) {
    function setMsgFromSon(){
      context.emit('get-msg','这是一条来自子组件的新的msg信息')
    }
   return { setMsgToSon } }
```

### 7.依赖注入

祖先组件向其所有子孙后代传递数据,不论嵌套层级有多深均可接收,传递的数据默认不是响应式的,需将要传递的数据变为响应式数据再传出

```js
import { provide,ref } from 'vue' // 祖先组件导入
 setup() {
 	// 先将要传递的数据变为响应式
    let name = ref(100)
    // 使用provide配置项注入数据 key - value
    provide('name', name)
  } 
// 后代组件接收
import { inject } from 'vue' // 导入函数
 setup() {
 	// 通过调用 inject 函数,传入 key 即可接收到传递过来的响应式数据
    const name = inject('name')
    return { name }
  }
```

### 8. 获取组件对象

在 vue2 中获取组件对象,一般是通过 ref

```js
<div ref="box"></div>
// 通过  this.$refs.box 获取 dom 对象,获取到后可以调用组件的数据和方法
// 通过 this.$refs.box.validate() 获取组件实例对象
// 在 vue3 中, 使用 ref 函数传入 null 创建 ref 对象, return 出去
// 模板中定义 ref 属性绑定,使用通过 ref 对象.value 
import { onMounted, ref } from 'vue' // 导入函数
setup() {
    const box = ref(null)
    onMounted(() => {
      console.log(box.value)
    })
    // 必须return
    return { box }
```

### 9. 一些非兼容语法

- eventBus : 该方法被移除,如需使用需借助第三方插件 mitt ,详见官网
- filter : 过滤器方法, 被移除,可用 methods 函数代替

```js
{{ msg | formatMsg }} // 被移除
{{ formatMsg('this is msg') }} // 使用函数代替
methods:{
  formatMsg(msg){
     return msg + 'zs'
  }
 }
```

- .sync : 修饰符语法,被移除, 和 v-model 语法合并

```js
<son v-model="cc"></son>
import { ref } from  'vue'
setup(){
	const cc=ref(50)
	return { cc }
}
// 子组件通过关键字 modelValue 接收, emit('update:modelValue', 值) 修改
 props: {
    modelValue: {
      type: Number,
      default: 0,
    }
  },
  setup(){
      const fn = () => {
        emit('update:modelValue', 222)
      }
      return { handleClick, fn }
  }
```



