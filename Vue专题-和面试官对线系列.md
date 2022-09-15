# 所有Vue知识点

## 1 Vue的特点

- 渐进式： 学一点，用一点， 可以不用学完全套东西，只要学习部分知识，就可以把Vue集成在我们的项目中，随着学习的深入，也可以使用Vue全家桶进行开发。

  -  可以在一个库和一套完整框架之间自如伸缩 

- 数据驱动，响应式数据：  只要改变数据， 自动更新渲染视图

- 双向数据绑定：  

  - 数据改变，自动更新视图
  - 表单输入数据， 自动把数据同步到data中

- 体积小， 压缩以后，vue整个库只有 20kb

- 性能高：不用操作dom，有虚拟dom，vue的底层帮助我们操作dom，比我们自己操作dom性能高

- 生态繁荣：  配套的周边资源非常丰富： 路由库、ui库， vuex插件...

  

## 2 Vue的环境搭建

### 2.1 vue-cli环境搭建

​	vue-cli是基于webpack写的

- 全局安装脚手架

  ```js
  yarn global add @vue/cli
  vue-cli --version   // 4.5.15 脚手架的版本
  ```

- 创建项目

  ```
  vue create 项目名
  (yarn create vite)
  中间，一堆选择（插件、版本、路由模式、css预处理、js/ts, 代码风格检测）
  yarn serve
  ```

### 2.2 vue-cli用到的加载器（loader）

- less-loader  css-loader style-loader  sass-loader stylus-loader // 打包css  和 css的预处理语言
- url-loader file-loader  raw-loader// 打包图片 文件 字体图标
- html-loader  // 打包html
- vue-loader // 打包.vue文件
- cache-loader // 做缓存
- vue-style-loader  // 打包.vue单文件组件中的， style中的样式
- postcss-loader // css兼容
- babel-loader // 编译es6 -》 es5
- eslint-loader // 处理eslint代码格式检测的

### 2.3 less和sass的区别

- 底层实现不一样
  - less基于javaScript的
  - sass是基于ruby的
- less和sass的语法不一样
  - 相同点
    - 嵌套
  - 不同
    - 混入方式不同
    - 声明变量不同
  - sass有两套语法
    - .sass 老语法
    - .scss 新语法 【推荐】

### 2.4 写自己的webpack配置

#### 2.4.1 使用方式

- 创建一个 `vue.config.js`的文件
- 在里面写的 webpack配置， 和 脚手架内置的webpack配置合并

#### 2.4.2 解决问题

- 配置自动打开浏览器

  ```js
  module.exports = {
      devServer: {
          open: true // 自动打开浏览器
      }
  }
  ```

- 配置代理跨域

  ```js
  # 什么是跨域？
  同源策略是浏览器的一种安全策略，要求： 域名、协议、端口，相同， 只要有一个不同，就是跨域, 如果跨域，浏览器会把ajax请求的数据拦截，接收不到数据。
  
  http://baidu.com  ---> https://baidu.com    跨域
  http://baidu.com  ---> http://jd.com  跨域
  https://baidu.com:8081  ---> https://baidu.com:8080 --> 跨域
  
  # 企业中主流解决方式
  1. Vue的 `vue.config.js` 中，配置代理
  // 新版本的脚手架 没有此文件 需要自己在项目根目录创建 写webpack配置 自动和内置的配置合并
  module.exports = {
      devServer: {
          port: 9050,
          open: true, // 自动打开浏览器
          // proxy: 'http://localhost:3000',
  
          proxy: {
              // 键名：地址
              '/api1': {
                  target: 'http://localhost:3000', // 服务器的目标 （你要发送ajax到的服务器）
                  changeOrigin: true, // 开启代理
                  ws: true, // 是否开启websocket
                  pathRewrite: {   // 去掉 路径中的  /api  的这一截
                      '^/api1': ''
                  }
              },
              '/api2': {
                  target: 'http://localhost:6000', // 服务器的目标 （你要发送ajax到的服务器）
                  changeOrigin: true, // 开启代理
                  ws: true, // 是否开启websocket
                  pathRewrite: {   // 去掉 路径中的  /api  的这一截
                      '^/api2': ''
                  }
              }
          }
      }
  }
  ```

- 其他跨域解决方式：

  - 古老的 jsonp [只是支持get方式]
    - 前端和后端同时配合
  - 后端设置 cros响应头 【主流】
  - 后端配置 nginx 【主流】



## 3 Vue的组件

### 3.1 .vue文件就是单文件组件

```vue
<template>
  1. 写html页面 模板
  2. 可以使用{{}}
  3. 可以使用指令
  4. 可以写自定义组件 <Hello />
  5. 只能有一个根标签
<h1 v-0905 class="title"></h1>
</template>

<script>
    // 写JS
    export default {
        data(){},
        component:{},
        props: {},
        methods: {},
        computed: {},
        watch: {},
        filter: {},
        mixins: [],
        生命周期系列 (8个)
    }
</script>

<style lang="less / sass / scss / stylus" scoped> // 让css只是对当前组件有效
    .title[v-0905] {
        color:red;
    }
</style>
```

### 3.2 自定义组件使用步骤

- 引入组件
- 注册组件
- 使用组件

### 3.3 用Vue写项目，本质就是写组件

我们一个组件，就是一个积木， 每个组件都有自己的： 页面、样式、js， 组件可以复用，组合，拼装成更大的页面，再大的项目，使用Vue开发，就是完成各种组件的编写。



## 4 Vue的模板表达式

```js
{{ 表达式 }}

表达式： 可以计算出唯一结果的一句代码

底层： 底层是如何实现的？？？
{{ message }}  --> 消息

正则替换 使用正则匹配 {{ message }} ==> 替换为data中对应的字段的值
```



## 5 Vue的指令

- v-text  
- v-html
- v-if    删除或重建dom
- v-show  切换display属性的值
- v-if v-else-if v-else
- v-for
- v-bind     ：
- v-on         @
- v-model   
- v-slot     #
- v-once   
- v-pre
- v-cloak

## 6 Vue的生命周期

初始化Vue到渲染的整个过程， 这个过程中会触发一系列的钩子函数， 作用：  给了我们添加自己代码的机会。

### 6.1 四个阶段 8个钩子函数

- 实例创建前后
  - beforeCreate
  - created  --> 发送ajax
- 挂载渲染前后
  - beforeMount
  - mounted  --> 操作dom
- 更新前后
  - beforeUpdate
  - updated
- 销毁前后
  - beforeDestroy  --> 解绑window上的全局事件 优化性能
  - destroyed

## 7 Vue的组件通信

组件之间，相互传递数据

### 7.1 父传子

- 主流（常用）---》 8种
  - 父组件把数据绑定在子组件标签属性上
  - 子组件通过props配置选项接收

### 7.2 子传父

- 子组件通过 this.$emit()触发一个自定义事件，把数据传递出来
- 父组件，通过 v-on （@）绑定自定义事件，然后再处理函数中，接收数据

### 7.3 bus中央事件总线

- 在main.js入口文件， new一个空的vue实例对象，把这个属性对象，挂载在Vue的原型上，一般叫$bus
- A组件通过 this.$bus.$emit()触发一个自定义事件，把数据传递出去
- B组件通过 this.$bus.$on()绑定一个自定义事件， 在回调函数中，接收数据

### 7.4 vuex

vuex是Vue的一个状态管理仓库，提供了5个核心API， 主要解决的问题是：

- 多个组件，共享数据
- 数据传递层级过深

#### 7.4.1 state

多个组件共享的数据，放入state中, 获取方式：this.$store.state

#### 7.4.2 mutations

修改state的唯一方式， 通过 this.$store.commit()修改状态 或 通过辅助函数 mapState修改状态

#### 7.4.3 actions

异步修改状态， 通过 this.$store.dispatc()触发， 或 通过辅助函数 mapActions 触发，最终还是需要调用mutations才能修改。

#### 7.4.4 getters

仓库中的计算属性computed， 获取方式：this.$store.getters 或 通过辅助函数 mapGetters

#### 7.4.5 module

拆分仓库，可以使用命名空间，把各个仓库拆分独立。



## 8 Vue的路由vue-router

路由的作用是让页面可以跳转，使用vue写的项目，都是spa单页面应用

### 8.1 路由模式

- 浏览器环境
  - hash模式  地址栏带#  
    - 底层基于 `onhashchange`
  - history模式  地址栏不带#
    - 底层基于 `pushState`
    - 缺点：  打包上线，如果刷新页面，会报错404， 需要后端配置重定向！！！！
- node环境
  -  abstract  【用不到！！！】

### 8.2 路由配置

```js
new Router({
    routes: [
        {path: '', component: 组件},
                               // 懒加载组件
        {path: '', component: () => import('组件路径')，children: [
             {path: '', component: 组件},
         ]}
    ]
})
```

### 8.3 路由对象

- $router

  路由实例对象， 提供了一些方法, 操作路由

  - push  跳转页面
  - back() 返回
  - go()  跳转页面
  - forward() 前进
  - replace() 替换 不会添加新的history

- $route

  路由信息对象，提供了一个属性，获取路由信息

  - path 路径
  - query 参数
  - params 参数
  - matched 当前匹配到的路由
  - meta  元信息

### 8.4 路由传参

- query  通过地址栏带参数 （注意：  不要带太多参数）

  - A组件跳转

    ```js
    this.$router.push({
        path: '路径',
        query:{
            id: 1,
            name: 'admin'
        }
    })
    ```

  - B组件获取

    ```js
    this.$route.query
    ```

- params （注意：不能刷新，多用于移动端）

  - A组件跳转

    ```js
    this.$router.push({
        name: '路由的名字',
        params:{
            id: 1,
            name: 'admin'
        }
    })
    ```

  - B组件获取

    ```js
    this.$route.params 
    ```

- 路由动态参数

  - A组件

    ```js
    this.$router.push({
        path: '/news/参数值'
    })
    ```

  - B组件

    ```js
    this.$router.params.参数名
    ```

  - 路由配置

    ```js
    {path: '/news/:参数名', component: News}
    ```

### 8.5 路由守卫 

- 全局前置守卫  （判断用户是否登录 拦截地址栏直接输入地址） 写全局

  ```js
  router.beforeEach((to, from, next) => {
    // to: 要去的目标路由对象
    // from: 要离开的路由对象
    // next: 放行
  })
  ```

- 全局后置守卫  （写全局）

  ```js
  router.afterEach((to, from) => {
    // to: 要去的目标路由对象
    // from: 要离开的路由对象
    // next: 放行
  })
  ```

- 路由独享守卫 (写路由配置中)

  ```js
  {
      path: "/about",
      name: "About", // 命名式路由
      component: () => import("../views/About.vue"),
      // 路由独享守卫
      beforeEnter: (to, from, next) => {
        console.log('进入路由之前',to, from, next)
        next()
      }
  }
  ```

- 组件内的守卫

  ```js
   beforeRouteEnter(to, from, next) {
   },
   beforeRouteUpdate(to, from, next) {
   },
   beforeRouteLeave(to, from, next) {
   }
  ```

  

## 9. Vue的配套ui框架

### 9.1 pc端

- elementui
- viewui ( ivew )
- ant design vue

### 9.2 移动端

- vantui



## Vue的内置API

### 更新数组的方法(数组变更方法)

 Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新 

Vue底层，不能挟持数组，它挟持了数组的方法，只要调用这些方法，也会触发视图更新

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

### ref

引用，相当于id，可以获取到dom 或 组件实例对象

```js
this.$refs.ref的值
```

### Vue.nextTick

和this.$nextTick功能是一样的，修改数据，可以获取到更新后的dom

```js
this.$nextTick(() => {
   // 更新数据 dom更新渲染完成 就会触发
   console.log(document.querySelectorAll('li')) 
})

Vue.nextTick(() => {
 // 更新数据 dom更新渲染完成 就会触发
 console.log(document.querySelectorAll('li')) 
})
```

### Vue.set

和``this.$set`是一样的，Vue初始化的时候，只会挟持data中的数据， 不会挟制数组， 所以，如果给对象添加新数据或修改数据的数据， 都不会触发视图更新，使用Vue.set可以重新挟持数据，让数据具备响应式。

```js
this.$set(对象, 属性名, 新的值)
Vue.set(this.user, 'email', 'dc@qq.com')

this.hobs.splice(1, 1, 'Vue')
this.$set(this.hobs, 1, 'React')
```

### Vue.directive

注册全局自定义指令， 我们就可以使用这些指令了

```js
Vue.directive('指令名', {
    // 配置对象 
    （当被绑定的元素插入到 DOM 中时 就会触发此函数）
    inserted: function (el, exp) {
        // 写这个指令的功能
    },
})
```

- 组件内的指令(局部的)

```js
 // 局部的
  directives: {
    border: {
      inserted (el, exp) {
        el.style.border = `${exp.value}px solid green`
      },
    },
  },
```

### Vue.filter

- 全局过滤器

  - filters/index.js

    ```js
    /**
     * 整个项目的全局过滤器
     */
    
    // 保留两位小数
    export function fixed2(val) {
        return val.toFixed(2)
    }
    
    // 首字母大写
    export function firstUpperCase(val) {
        return val.slice(0, 1).toUpperCase() + val.slice(1)
    }
    
    // 数字千分位逗号
    export function toThousand (val) {
        val += ''
        return val.replace(/(?=(?!\b)(\d{3})+$)/g, ",");
    }
    
    ```

    

  - main.js

  ```js
  Object.keys(filters).forEach(key => {
    // 定义全局过滤器
    Vue.filter(key, filters[key])
  })
  ```

  

- 局部过滤器（组件内的）

  ```js
   filters: {
        fixed2(val) {
          return val.toFixed(2)
        }
      }
  ```

  

### Vue.component

- 局部组件

  ```js
  export default {
      components: {
          // 在这里注册
      }
  }
  ```

- 全局组件 （一旦注册 哪里都可以使用）

  ```js
  main.js
  Vue.component('组件标签', 组件)
  ```

### Vue.use

安装Vue的插件， 只要是基于Vue的插件，都需要use一下

### 配置选项

- data

  - data是存放当前组件内数据的， data必须是一个函数，然后返回一个对象。

  -  **一个组件的 `data` 选项必须是一个函数**，每个实例可以维护一份被返回对象的独立的拷贝 

    简单的说： 如果是对象，就会共享数据，如果是函数，使用这个组件的时候，返回的是一个新的独立的对象，没有共享数据的问题。

- props

  - 接收父组件的数据

    - 写法一

      ```js
      props: ['属性1', '属性2']
      ```

    - 写法二

      ```js
      props: {
          属性1：{
              type: String | Boolean | Number | Object | Array, // 限制类型
              required:true // 是否必填
              default: 默认值
          },
          属性2：{
              type: String | Boolean | Number | Object | Array, // 限制类型
              required:true // 是否必填
              default() {
                  return [] | {}
              }
          }
      }
      ```

- computed

  - 特点

    - 计算属性，主要用于一堆逻辑计算，最终返回一个唯一结果，必须有返回值。
    - 计算属性写法和methods是一样的，使用函数名，就代表返回结果
    - 有依赖缓存，只要依赖的响应式数据发生变化，就会重新计算， 性能高

  - 写法

    - 写法一 （标准写法 只是取数据）

      ```js
      name() {
          return this.firstName + ' ' + this.lastName
      }
      ```

      

    - 写法二（有get和set 可以获取 也可以修改）

      ```js
      name: {
        get() {
          return this.firstName + " " + this.lastName;
        },
        set(newVal) {
          let t = newVal.split(' ')
          this.firstName = t[0]
          this.lastName = t[1]
        },
      }
      ```

- methods

  - 方法，主要用于绑定事件调用
  - 特点
    - 不一定有返回值的，看这个方法的需求

- watch

  - 侦听器， 主要用于观察数据变化，执行关联操作，自动传入新数据和老数据

    ```js
    
      // 侦听器
      watch: {
        num1(newVal, oldVal) {
          console.log("num1变化就会触发", newVal, oldVal);
        },
        num2: "show",
        num3: {
          handler(newVal, oldVal) {
            console.log("num3变化就会触发", newVal, oldVal);
          },
          // 其他的配置选项
          immediate: true, // 立即执行
          // deep: true, // 深度监听
        },
        user: {
          // 处理函数 观察到数据变化之后 处理的函数
          handler(newVaL, oldVal) {
            console.log("user变化了", newVaL, oldVal);
          },
          deep: true, // 开启深度监听
        },
      },
    ```

    

- el   挂载dom  --》 把Vue当成js库用

- template  页面模板  --》 把Vue当成js库用

- render：  渲染函数 里面可以写 jsx 使用h函数（createElement的函数），创建页面

### 插槽

子组件预留位置，父组件插入内容

- 分类
  - 具名插槽
  - 匿名插槽
  - 作用域插槽



## 实例属性

$data：  获取数据 获取到的是 data返回的对象

$el：  获取dom

$parent：  获取父组件

$refs：  获取dom  

$attrs：  获取属性（props没有接收的）

## MVC & MVVM

MVC和MVVM都是架构模式。

- MVC （ backbone.js ）[了解]

  - M: Model数据模型

  - V:  View 视图

  - C: controller控制器

    数据模型发生改变，交给控制器处理，控制器通知视图渲染

- MVVM

  - M: Model数据模型
  - V： View视图
  - VM： ViewModel 视图模型

   Vue是基于MVVM实现的，主要是基于MVVM实现了双向数据绑定。

  - 数据-》视图：  使用Object.defineProperty递归挟持Vue实例对象data中的所有数据，对所有数据的访问就getter和setter代理了，当我们改变数据，就会触发setter函数，Vue的底层就会通知视图更新渲染。

  - 视图-》数据： Vue对表单的输入进行了dom监听，把监听到的数据，同步到Model数据模型中（data中）

    

- 数据的响应式原理

  - 数据-》视图： 响应式就是我们只要修改数据，自动更新渲染视图，原理是： 使用Object.defineProperty递归挟持Vue实例对象data中的所有数据，对所有数据的访问就getter和setter代理了，当我们改变数据，就会触发setter函数，Vue的底层就会通知视图更新渲染

- 观察者模式：设计模式 【了解】

  ​	初始化代码的时候，Vue的底层有一个observe把所有数据都挟持了， 有一个订阅者Dep和一个观察者Watcher，Dep实现把访问过的属性收集起来了，当数据改变，触发setter，Dep通知观察者，说数据变化了，观察者调用更新方法，进行数据的更新，通知视图进行渲染。

## key

Key是一个节点的唯一标识，用来识别节点，主要是： 虚拟dom和真实dom对比，有key才能识别。

 `key` 的特殊 属性 主要用在 Vue 的虚拟 DOM 算法，在新旧 节点 对比时辨识 VNodes ，key尽量不用数组的索引，不稳定。

## 动态组件&异步组件

- 动态组件

  ```js
  <component :is="JlForm"></component>
  
  export default {
      data() {
          return {
            JlForm
          }
       },
  }
  ```

- 异步组件

  ```js
   components: {
      // JlButton,
      // 异步组件
      'jl-button': () => import('@/components/JlButton.vue')
    },
  ```

  

## 服务器端渲染[了解]

Vue不在浏览器端渲染，而是在后端（服务器端进行模板解析渲染）

- 优点：
  - 性能高 （浏览器解析模板，肯定比不上服务器解析模板块）
  - 好做seo （搜索引擎优化），浏览器端渲染，vue是spa单页面应用，不利于seo
- 使用vue做服务器端渲染有两种方案
  - ssr   serve side render  一套解决方案，把vue由前端渲染变成后端渲染
  - nuxt.js   一个框架 专门基于vue 来做服务器端渲染的

## 内置组件

- keep-alive

- slot

- component

- transition 动画

  ```js
   <!-- 内置的动画组件 把我们要做动画的组件包裹起来 -->
      <transition name="fade">
        <jl-model @close="visible=false" v-show="visible"></jl-model>
      </transition>
  
  // 进入动画的过程 和 离开动画的过程
  .fade-enter-active, .fade-leave-active {
    // 过渡
    // transition: opacity 5s;
    transition: all 0.3s;
  }
  
  // 进入的瞬间 离开的瞬间
  .fade-enter, .fade-leave-to  {
    // opacity: 0;
    transform: translateX(-100%);
  }
  ```

  

## 修饰符

- 事件处理修饰符

  - 事件修饰符
    - stop  阻止冒泡   （等同于 e.stopPropagation()）
    - prevent  阻止默认行为 ( 等同于 e.preventDefault() )
  - 按键修饰符
    - enter  回车
    - space  空格
    - 按键码 ，是一个数字（KeyCode）

- 表单输入修饰符，搭配v-model使用

  - .lazy    把input事件改为change事件
  - .number   转为数字类型
  - .trim  去除首尾空格

- 自定义事件修饰符

  - .sync  主要是用于实现双向绑定 （这个属性父传子，但是也允许子组件修改，只是一种缩写）

    ```js
    <jl-xx :visible.sync="visible"></jl-xx>
    
    <jl-xx @update:visible="visible=$event" :visible="visible"></jl-xx>
    ```

## 自定义v-model

在自己封装的组件上，可以使用v-model进行数据的双向绑定。

v-model只是缩写：  名为input的事件 和 名为 value的属性

```js
<jl-input-number v-model="num"></jl-input-number>

<jl-input-number :value="num" @input="num=$event"></jl-input-number>

<el-rate v-model="value1"></el-rate>
<el-rate :value="value1" @input="value=$event"></el-rate>
```



## 混入 mixin

把公共的配置选项，抽取出去，然后，可以合并进入任务的组件内部。

作用：  抽取公共配置项

缺点： 混乱，难以维护，很垃圾。

mixins/index.js

```js
/* 
    混入的公共选项
*/

const globalMixins = {
    data() {
        return {
            page: 1,
            total: 10
        }
    },

    created() {
        console.log('666')
    },
    methods: {
        hello() {
            return 'Hello 你好'
        }
    }
}

export default globalMixins
```

- About.vue

```js
export default {
     // 混入
  mixins: [globalMixins],
}
```

## 递归组件 [了解]

组件内部，自己使用自己，就是递归组件，常见的就是： 菜单树，

注意：组件要给name，才可以。



组件给name的作用：

- 递归组件，可以自己在模板中使用自己
- keep-alive缓存组件



## keep-alive

缓存组件，切换到这个组件的时候，不会重新渲染

```js
<keep-alive>
    <组件 include=['组件名1'， '组件名2'] exclude=['组件名3','组件名5'] :max="100"></组件>
</keep-alive>
```



## 虚拟DOM

虚拟dom其实就是js的一个对象，用来描述DOM的结构，数据改变，渲染页面，vue的底层会创建一个虚拟的dom，然后虚拟dom和真实dom对象对比（ diff算法 -》 比较不同 ），只会重新渲染不同的地方（最小粒度更新）。



# 项目相关的

## token

作用： 接口鉴权

- 登录成功，后端给token令牌
- 请求拦截器，带上token
- 如果有token，就可以拿到数据，否则，后端返回401，未授权，拿不到数据

## 权限

- 登录成功，后端返回角色role
- 把路由一分为2： 静态路由， 动态路由，动态路由配置meta字段
- 根据role，计算出有权限访问的路由，然后使用addRoutes方法，添加到路由实例中
- 根据动态路由，计算菜单，动态渲染菜单
- 坑：
  - 退出系统，刷新页面 （路由只有addRoutes添加方法，没有删除方法，刷新才可以重新计算）



## 请求拦截和响应拦截

- 请求拦截
  - 请求的时候，统一携带某个字段，例如token
- 响应拦截器
  - 对状态码进行统一处理，例如弹窗



## 响应式布局

- 使用媒体查询，进行响应式布局， 不同的屏幕宽度，写不同的样式效果

  ```js
  @media only screen and (max-width: 500px) {
      // 里面的样式 对 屏幕宽度 小于500px生效
      .gridmenu {
          width:100%;
      }
  
      .gridmain {
          width:100%;
      }
  
      .gridright {
          width:100%;
      }
  }
  
  @media only screen and (min-width: 500px) and (max-width: 1000px) {
      // 里面的样式 对 屏幕宽度  500~1000 生效
      .gridmenu {
          width:100%;
      }
  
      .gridmain {
          width:100%;
      }
  
      .gridright {
          width:100%;
      }
  }
  
  @media only screen and (min-width: 1000px) {
      // 里面的样式 对 屏幕宽度  大于1000 生效
      .gridmenu {
          width:100%;
      }
  
      .gridmain {
          width:100%;
      }
  
      .gridright {
          width:100%;
      }
  }
  ```



## 支付

- 【1】用户登录，通过wx.login得到code

- 【2】发送code给后端，得到 openId

- 【3】发送下单请求，携带openId， 得到支付关键参数： 预支付id，签名，随机串

- 【4】调用支付方法 uni.requestPayment()， 调起支付界面

- 【5】发送ajax，查询支付结果

  

## echarts封装

- 编写一个echart组件

- 接收参数 ： 配置项

- 使用侦听器，监听配置项，如果配置项发生变化，开始重新画图

  ```js
  export default {
      props: {
          // 配置项不是写死的 而是传入的 用户传入什么配置 就生成什么图表
          options: {type: Object}
      }，
      
      watch: {
        options: {
           handler() {
               this.drawPie() // 如果options变化 重新调用画图的方法
           },
           deep: true // 开启深度监听
        }
      }
  }
  ```

  

## 分享

在声明周期：   onShareAppMessage , 写分享代码

## axios封装

- 工具函数层：  整个项目封装一个通用的请求工具函数 request.js

- ajax接口管理层：  创建一个api文件夹，里面，一个js文件，负责写一个模块的所有ajax函数

- 组件中，引入自己需要的ajax函数，调用此函数，发送请求

  

## 性能优化

- 传统
  - 减少http请求 （ 本地存储 ）
  - 使用精灵图
  - 懒加载
  - 减少dom操作 （框架是数据驱动的，我们不需要操作dom）
  - 防抖
  - 节流

- Vue

  - 组件，第三方库，按需引入

  - 在beforeDestory解绑window事件和销毁定时器

  - 组件复用

  - CDN 在线资源， 不用下载安装到页面，打包速度快，打包体积小

  - 使用字体图标

  - 使用keep-alive缓存组件

  - v-for加上key，key一般用id

  - 异步组件（组件的懒加载）和异步路由（路由懒加载）

  - SSR 提升首屏渲染速度（ 把组件防在服务器端渲染 ）

  - 图片懒加载

  - 开启代码压缩 （脚手架打包的代码已经压缩了）

    

# Vue3 【了解】

- Vue3底层代码，全部使用TypeScript重构，性能提升20%以上

- Vue挟持数据 Object.defineProperty -> proxy代理，挟持更彻底。

- Vue重写响应式系统，数据响应式写法改变了   **（响应性API）**  ref reactive

- Vue3脚手架提供两个版本：  webpack( @vue/cli )   vite（尤雨溪新写的打包工具和webpack一样）

  大型项目，@vue/cli, 写完代码，刷新，可能要等10多秒，vite秒开

- compositionAPI **组合式api**    setup

  - V2:  配置式api ( 内置配置选项： data props component computed watch methods。。。 )
  - V3:  组合式api , setup  (v3支持v2写法， 不建议混写)

  

  

