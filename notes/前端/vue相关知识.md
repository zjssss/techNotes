### 架构模式

#### mvc

MVC：Model   View   Controller

MVC模式的意思是，软件工程可以分成三个部分，进行单向数据流

- 视图（View）：用户界面。
- 控制器（Controller）：业务逻辑；在Controller里面把Model的数据赋值给View来显示（或者是View接收用户输入的数据然后由Controller把这些数据传给Model来保存到本地或者上传到服务器）。
- 模型（Model）：数据保存，获取数据的地方

通信方式：所有通信都是单向的

1. View 传送指令到 Controller
2. Controller 完成业务逻辑后，要求 Model 改变状态
3. Model 将新的数据发送到 View，用户得到反馈

缺点：Controller里面的代码非常臃肿且难以维护

#### mvvm

MVVM：Model、View、ViewModel

主要就是MVC中Controller演变成MVVM中的viewModel

 MVVM与MVC最大的区别就是：它实现了View和Model的自动同步，是双向数据流

**优点：**

1. 双向绑定技术，当Model变化时，View-Model会自动更新，View也会自动变化。很好的做到数据的一致性

2. 由于控制器的功能大都移动到View上处理，大大的对控制器进行了瘦身

3. View的功能进一步强化，具有控制的部分功能

**缺点**

1. 数据绑定也使得bug很难被调试。比如你看到页面异常了，有可能是你的View的代码有bug，也可能是你的model的代码有问题。数据绑定使得一个位置的Bug被快速传递到别的位置，要定位原始出问题的地方就变得不那么容易了。

2. 数据双向绑定不利于代码重用。客户端开发最常用的是View，但是数据双向绑定技术，让你在一个View都绑定了一个model，不同的模块model都不同。那就不能简单重用view了

3. 一个大的模块中model也会很大，虽然使用方便了也很容易保证数据的一致性，但是长期持有，不释放内存就造成话费更多的内存。

#### mvvm双向绑定

（1）发布订阅者的设计模式: 一般通过发布和订阅消息的方式实现数据和视图的绑定监听，更新数据方式通常做法是 vm.set(‘property’, value)。

（2）脏值检查: angular.js 是通过脏值检测的方式比对数据是否有变更，来决定是否更新视图，在指定的事件触发时进入脏值检测。

（3）数据劫持: vue.js 则是采用数据劫持结合发布订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

### vue的指令

#### 常用指令

v-if，v-show，v-for，v-bind，v-on，v-model，v-html，
v-once（只渲染一次，后面元素中的数据再更新变化，都不会重新渲染）

#### 自定义指令

（1）对普通DOM元素进行底层操作，这时候就会用到自定义指令
（2） 自定义指令里的钩子函数
 bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置
 inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中
 update：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新。
（3） 常用钩子函数的参数
 el：指令所绑定的元素，可以用来直接操作DOM 。
 binding：一个对象，包含以下属性：
 name：指令名，不包括 v- 前缀。　　
 value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。

#### 几种自定义指令的使用

 自定义一个 v-focus 指令，即在 <input> 或 <textarea> 标签初始化时获得焦点

```js
Vue.directive('focus', {
    inserted: function (el) {
        el.focus();//聚焦
    }
});
<input type="text" v-focus>
```

自定义一个权限指令，对需要权限判断的 Dom 进行显示隐藏

```js
function checkArray(key) {
  let arr = ['1', '2', '3', '4']
  let index = arr.indexOf(key)
  if (index > -1) {
    return true // 有权限
  } else {
    return false // 无权限
  }
}
 
const permission = {
  inserted: function (el, binding) {
    let permission = binding.value // 获取到 v-permission的值
    if (permission) {
      let hasPermission = checkArray(permission)
      if (!hasPermission) {
        // 没有权限 移除Dom元素
        el.parentNode && el.parentNode.removeChild(el)
      }
    }
  },
}
 
export default permission

//给 v-permission 赋值判断即可

<div class="btns">
  <!-- 显示 -->
  <button v-permission="'1'">权限按钮1</button>
  <!-- 不显示 -->
  <button v-permission="'10'">权限按钮2</button>
</div>
```

自定义一个防抖函数的指令

```js
const debounce = {
  inserted: function (el, binding) {
    let timer
    el.addEventListener('click', () => {
      if (timer) {
        clearTimeout(timer)
      }
      timer = setTimeout(() => {
        binding.value()
      }, 1000)
    })
  },
}

export default debounce

<button v-debounce="debounceClick">防抖</button>

```

### 用函数的方式调用组件

```vue
<template>
  <div class="custom-popover" :class="popoverClass" :style="{width: (width / 192) + 'rem'}">
    <div class="popover-title">
      <span>{{title}}</span>
      <span class="el-icon-close" @click="closeWindow"></span>
    </div>
    <div class="popover-content">
      <component :is="pageValue" :params='params'></component>
    </div>
  </div>
</template>
```

```js
import Vue from 'vue';
import popover from './popover.vue';
const getPopover = Vue.extend(popover);
export default $popover = function(obj){
	const getCon = new getPopover(obj).$mount();
    document.body.appendChild(getCon.$el);
    
}
getCon.$el.parentNode.removeChild(getCon.$el);
```

```js
import showPopover from './$popover';
showPopover(obj)
```



### this.$set

当在对象或者数组添加新的属性，视图没有更新时，可以使用这个api

```js
//对象 this.$set(object, 'key', value)
this.$set(this.ctrPointMadeUp, 'point', res.data.data.focusProportion)
//数组  this.$set(array, index, item)  this.$set(array[index],"key", value)
this.$set(this.items, index, item)
this.$set(this.items[index],"flag", !item.flag)
```

### 生命周期

#### 概述

  vue是通过构建数据驱动的web界面的渐进式框架
 vue生命周期就是指vue实例从创建到销毁的过程

#### 过程

 beforeCreate：实例已经初始化，只是数据观察与事件机制尚未形成，不能获取DOM节点
 created：实例已经创建，仍然不能获取DOM节点（有data，没有el）
 beforeMount：是个过渡阶段，获取不到具体的DOM节点，但是vue挂载的根节点已经创建（有data，有el）
 mounted：数据和DOM都已经被渲染出来了
 beforeUpdate和updated：data中的数据发生了改变，会触发对应组件的重新渲染，先后调用beforeUpdate和updated
 beforeDestroy和destroyed：vue实例即将销毁和销毁时的调用

#### 父子组件的生命周期

加载渲染过程
父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted
子组件更新过程
父beforeUpdate->子beforeUpdate->子updated->父updated
父组件更新过程
父beforeUpdate->父updated
销毁过程
父beforeDestroy->子beforeDestroy->子destroyed->父destroyed

### 组件传值

主要分为两类：1.父子组件间的传值   2.非父子组件间的传值

1 父组件使用v-bind进行数据的绑定，子组件中通过定义props接收对应的数据
2 使用$children和$parents获取子组件和父组件对象

**$children** 指代的是子组件，返回的是一个组件集合

**$parent** 指代的是父组件 ，返回的是一个组件集合

```js
<template>
<div>
  <button @click="parentClick">点击访问父组件</button>
</div>
</template>
<script>
export default {
  data(){
    return {
      msg:"test1"
    }
  },
  methods: {
    parentClick(){
        this.$parent.funa("xx")
       console.log(this.$parent.total);
    }
  }
}
</script>
```

3 使用$ref获取指定的子组件

```js
<template>
<div>
  <testVue ref="test"></testVue>   
  <testVue2></testVue2> 
  <button @click="clickChild1">$refs方式获取子组件值</button> 
  <button @click="clickChild2">$children方式获取子组件值</button>
</div>
</template>
<script>
import testVue from './test'
import testVue2 from './test2'
export default {
  data(){
    return {
      total: 108
    }
  },
  components: {
    testVue,
    testVue2  
  },
  methods: {
    funa(e){
        console.log("index",e)
    },
    clickChild1(){
      console.log(this.$refs.test.msg);
    },
    clickChild2(){
        console.log(this.$children[0].msg);
        console.log(this.$children[1].msg);
    }
  }
}
</script>
```

4 eventbus事件总线
5 vuex
6 路由传值

### vue的路由

#### 常用配置项

path:路由请求的路径
component:路径匹配成功后需要渲染的组件或者页面
redirect:重定向
children:路由嵌套
name:命名路由 给当前路由取一个别名
props:路由解耦 路由传参的一种方式 针对动态路由
meta:路由元信息 当前路由所携带的一些信息

#### 路由懒加载

异步组件require()方法
es6import方法
webpack提供的require.ensure()

```js
{ component: resolve=require(['@/components/home'],resolve) }
{component: () => import("@/components/HelloWorld")}
```

#### 路由传参的区别

query刷新不会丢失query里面的数据，params刷新 会 丢失 params里面的数据
query在浏览器地址栏中显示参数，params不显示

#### 路由守卫

router.beforeEach 是页面加载之前，相反router.afterEach是页面加载之后

**前置路由**

路由白名单

利用token或cookie判断是否能跳转到首页或者重定向登录页

**后置路由**

跳转后的页面例如滚动条回调0 0 位置

更新页面title

懒加载结束等等

#### 不同路由跳转的区别

router-link默认是触发router.push(location)，如果设置的replace 则触发router.replace(location)



router.push() ：导航跑到不同的URL,这个方法会向history栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的url.

router.replace(): 跟router.push作用是一样的，但是，它不会向history添加新记录，而是跟它的方法名一样替换掉当前的history记录.

router.go(n): 这个方法的参数是一个整数，意思是在history记录中向前或者后退多少步，类似window.history.Go(n) 

#### hash路由和history路由

 hash模式url带#号，可以用window.location.hash 读取，也可以用onhashchange判断url的变化
history模式不带#号，history路由是HTML5新增的API（history.pushState）

我们进行回车刷新操作，hash路由会加载到地址栏对应的页面，而history路由一般就404报错了（刷新是网络请求，没有后端准备时会报错）



当我们进入项目的主页的时候，一切正常，可以访问，但是当我们刷新页面或者直接访问路径的时候就会返回404，那是因为在history模式下，只是动态的通过js操作window.history来改变浏览器地址栏里的路径，并没有发起http请求，但是当我直接在浏览器里输入这个地址的时候，就一定要对服务器发起http请求，但是这个目标在服务器上又不存在，所以会返回404，怎么解决呢？我们现在可以把所有请求都转发到 http://localhost:8080/bank/page/index.html上就可以了。

#### 路由权限

公共路由

从根据用户信息接口访问一个路由信息接口，或者用户信息接口自带路由信息

```js
import { constantRoutes } from '@/router'
import error from '@/router/error'
import { getRouters } from '@/api/menu'
import Layout from '@/layout/index'

const permission = {
  state: {
    routes: [],
    addRoutes: []
  },
  mutations: {
    SET_ROUTES: (state, routes) => {
      state.addRoutes = routes
      state.routes = constantRoutes.concat(routes)
    }
  },
  actions: {
    // 生成路由
    GenerateRoutes({ commit }) {
      return new Promise(resolve => {
        // 向后端请求路由数据
        getRouters().then(res => {
          let accessedRoutes = filterAsyncRouter(res.data)
          accessedRoutes = accessedRoutes.concat(error)
          commit('SET_ROUTES', accessedRoutes)
          resolve(accessedRoutes)
        })
      })
    }
  }
}

// 遍历后台传来的路由字符串，转换为组件对象
function filterAsyncRouter(asyncRouterMap) {
  return asyncRouterMap.filter(route => {
    if (route.component) {
      // Layout组件特殊处理
      if (route.component === 'Layout') {
        route.component = Layout
      } else {
        route.component = loadView(route.component)
      }
    }
    if (route.children != null && route.children && route.children.length) {
      route.children = filterAsyncRouter(route.children)
    }
    return true
  })
}

export const loadView = (view) => { // 路由懒加载
  return (resolve) =>  require([`@/views/${view}`], resolve)
}

export default permission
```

在前置路由守卫使用addrouter动态添加api返回的路由信息实现路由权限

```js
  store.dispatch('GenerateRoutes', { roles }).then(accessRoutes => {
          // 测试 默认静态页面
          // store.dispatch('permission/generateRoutes', { roles }).then(accessRoutes => {
            // 根据roles权限生成可访问的路由表
            router.addRoutes(accessRoutes) // 动态添加可访问路由表
            next({ ...to, replace: true }) // hack方法 确保addRoutes已完成
          })
```

#### 路由白名单

在前置路由守卫进行路由白名单的判断

```js
const whiteList = ['/login', '/auth-redirect', '/bind', '/register']

if (whiteList.indexOf(to.path) !== -1) {
      // 在免登录白名单，直接进入
      next()
    } 
```



### computed和watch的区别

computed 计算属性 : 依赖其它属性值，并且 computed 的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed 的值。

watch 侦听器 : 更多的是「观察」的作用，无缓存性，类似于某些数据的监听回调，每当监听的数据变化时都会执行回调进行后续操作。

需要在数据变化时执行异步或开销较大的操作时，应该使用 watch，使用 watch 选项允许我们执行异步操作 ( 访问一个 API ),限制我们执行该操作的频率,并在我们得到最终结果前,设置中间状态。这些都是计算属性无法做到的。

当我们需要进行数值计算，并且依赖于其它数据时，应该使用 computed，因为可以利用 computed 的缓存特性，避免每次获取值时，都要重新计算。

### 计算属性的setter和getter

当页面获取某个数据的时候，先会在data里面找，找不到就会去计算属性里面找
在计算属性里面，获取的时候会自动去执行get方法
//父组件传递属性，子组件中不能直接修改，所以使用computed的setter触发父组件事件

```js
computed: {
    isShowData: {
      get() {
        return this.isShow;
      },
      // setter，触发父组件更新isShow
      set() {
        this.$emit("update:isShow", false);
      },
    },  
  },
```

### watch和computed的原理

#### computed

1. 调用计算属性时会触发其`Object.defineProperty`的`get`访问器函数
2. 当某个属性发生变化，触发`set`拦截函数，然后调用自身消息订阅器`dep`的`notify`方法，遍历当前`dep`中保存着所有订阅者`wathcer`的`subs`数组，并逐个调用`watcher`的 `update`方法，完成响应更新

### watch中的deep:true是如何实现的

当用户指定了watch中的deep属性为true时，如果当时监控的属性是数组类型，会对对象中的每一项进行求值，此时会将当前watcher存入到对应属性的依赖中，这样数组中对象发生变化时也会通知数据更新。
内部原理就是递归，耗费性能

### 双向绑定（vue源码）

#### 原理

是通过数据劫持+订阅发布者模式实现的
数据劫持：指的是在修改或者访问某个对象的属性时，通过代码拦截劫持，然后进行修改返回这个最后的结果
订阅发布者模式：对象数据之间存在着一对多的相互依赖对应关系，当一个数据发生改变时，所有的依赖对象都会收到通知。
vue2.x使用Object.defineProperty();（有缺陷，无法检测到对象属性的新增或删除，不能监听数组的变化）
vue3.x使用es6的Proxy;

#### 区别

Proxy代理整个对象，Object.defineProperty只代理对象上的某个属性，所以遍历对象的每个属性

对象上定义新属性时，Proxy可以监听到，Object.defineProperty监听不到。

数组新增删除修改时，Proxy可以监听到，Object.defineProperty监听不到

#### 几种方式

**1  v-model**

```js
//父组件
<BoxTabs v-model="currentTabs" :headerList="headerList"></BoxTabs>  
//子组件
<div :class="{'active':index == currentTab}" >{{ item }}</div>
 props: {
    value: {
      type: Number,
      default: 0
    },
  },

watch: {
    value(val) {
      this.currentTab = val;
    },
    currentTab(val){
        this.$emit('input',val);
    }
  },
data() {
    return {
      currentTab: this.value,
    };
  },
```

**方式二  .sync  this.$emit('update:msg', Data)**

```js
//父组件
<view :msg.sync="msg"></view>

//子组件
<button @click="childBtn">儿子</button>

props: {
   msg: {
      type: String,
      default: ''
    },
  },

methods:{
    childBtn(){
    this.$emit('update:msg', Data)
              },
},
```


### vue2和vue3的区别

Vue.js是一个提供MVVM数据双向绑定的库，专注于UI层面，核心思想是：数据驱动、组件系统

```
双向绑定使用了 proxy 替代 Object.defineProperty，提升内存性能
新增 hook api
Composition API（组合API）
更好的ts支持，vue 3 直接用 ts 重写。
部分命令发生了变化
```

### Vue 3 性能提升的方面

```
1 响应式系统提升
vue2在初始化的时候，对data中的每个属性使用definepropery调用getter和setter使之变为响应式对象。
如果属性值为对象，还会递归调用defineproperty使之变为响应式对象
vue3使用proxy对象重写响应式。proxy的性能本来比defineproperty好
proxy可以拦截属性的访问、赋值、删除等操作，不需要初始化的时候遍历所有属性
优势：
可以监听动态新增的属性；
可以监听删除的属性 ；
可以监听数组的索引和 length 属性；
2  编译优化
优化编译和重写虚拟dom，让首次渲染和更新dom性能有更大的提升
vue2 通过标记静态根节点,优化 diff 算法
vue3 标记和提升所有静态根节点,diff 的时候只比较动态节点内容
3 源码体积的优化
vue3移除了一些不常用的api，例如：inline-template、filter等
```

### Composition Api 与Options Api 的区别

```
Options Api
包含一个描述组件选项（data、methods、props等）的对象 options；
API开发复杂组件，同一个功能逻辑的代码被拆分到不同选项 ；
使用mixin重用公用代码，也有问题：命名冲突，数据来源不清晰；
composition Api
vue3 新增的一组 api，它是基于函数的 api，可以更灵活的组织组件的逻辑。
解决options api在大型项目中，options api不好拆分和重用的问题。
```

### **vue 中容易出现内存泄露的几种情况**

声明的全局变量在切换页面的时候没有清空(在页面卸载的时候顺便处理掉该引用 window.test = null )
监听在 window/body 等事件没有解绑。（在页面销毁的时候，顺便解除引用，释放内存）
绑在 EventBus 的事件没有解绑（在页面卸载的时候也可以考虑解除引用）
v-if 指令产生的内存泄露


### vue手动挂载dom节点

```js
//MyComponent 就是引入的vue文件
var component = new MyComponent().$mount()
document.getElementById(‘app‘).appendChild(component.$el)
```

### vue本地代理跨域

```js
   devServer: {
       proxy: {
         '/video': {
           target: 'http://113.98.62.19:8088/',
           ws: false,
           changeOrigin: true
         }
      }
     },
```

### 事件总线

#### 写法描述

```js
// 创建写法1：
let EventBus = new Vue() //vue实例可以作为事件总线
Object.defineProperties(Vue.prototype,{
    $bus:{
       get(){
        return EventBus  
    }   
  }
})
// 创建写法2：
Vue.prototype .$bus = new Vue()

//发射事件
 EventBus.$emit("aMsg", '来自A页面的消息');
//接收事件
EventBus.$on("aMsg", (msg) => {
      // A发送来的消息
      this.msg = msg;
    });
//移除事件
EventBus.$off('aMsg')
```

#### 事件总线的弊端

```json
在上一个例子中，我们A组件向事件总线发送了一个事件aMsg并传递了参数MsgA，然后B组件对该事件进行了监听，并获取了传递过来的参数。但是，这时如果我们离开B组件，然后再次进入B组件时，又会触发一次对事件aMsg的监听，这时时间总线里就有两个监听了，如果反复进入B组件多次，那么就会对aMsg进行多次的监听。

总而言之，A组件只向EventBus发送了一次事件，但B组件却进行了多次监听，EventBus容器中有很多个一模一样的事件监听器这时就会出现，事件只触发一次，但监听事件中的回调函数执行了很多次

- 解决办法：在组件离开，也就是被销毁前，将该监听事件给移除，以免下次再重复创建监听
- 语法：`this.$EventBus.$off(要移除监听的事件名)`
```

### vuex

index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
export default new Vuex.Store({
  modules: {
    app,
    transferData,
    dataRecord
  }
})
```

子文件

```js
export default {
    state: {
        disCreenImg:{},
    },
    mutations: {
      setDisCreenImg(state,imgObj){
        state.disCreenImg=imgObj;
      },
    },
    getters: {
     // 单个参数
        countDouble: function(state){
          return state.count 
        },
        // 两个参数
        CountDoubleAndDouble: function(state, getters) {
          return getters.countDouble 
        }
    },
    actions: {
      //
    }
  };
```

#### 基本使用

```js
this.$store.state.animalChange
this.$store.commit('setAnimalChange',{});
this.$store.dispatch('onLogin')
```

getter相当于vuex的计算属性，getter的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

```js
computed(){   
countDouble: function(){
   return this.$store.getters.countDouble
 }
}

  import { mapGetters } from 'vuex'
 ...mapGetters([
      'countDouble',
      'CountDoubleAndDouble',
    ]),
```

#### action和mutation区别

mutation是同步更新数据（内部会进行是否为异步方式更新的数据检测）

action异步操作，可以获取数据后调用mutation提交最终数据

### 在使用计算属性时，函数名和 data 数据源中的数据可以同名吗？

 不可以重名，不仅仅是计算属性和 data，其他的如 props，methods 都不可以重名，因为 Vue 会把这些属性挂载在组件实例上，直接使用 this 就可以访问，如果重名就会导致冲突。

### 怎么给 vue 定义全局的方法

目前一般有两种做法：一是给直接给 Vue.prototype 上添加，另外一个是通过 Vue.mixin 注册



### vue引入百度地图

```js

<template>
  <div class="baidu_map" id="baidu_map"></div>
</template>

<script>
export default {
  name: "Bmap",
  props: {
    // point:{
    //   type:Array,
    //   default:[114.051549, 22.557686],
    // }
  },
  data() {
    return {
      bmap: undefined,
      point: [114.051549, 22.557686],
      level: 17,
      imgIcon: null
    };
  },
  methods: {
    // 初始化地图
    onReady() {
      let styleJson = require("./mapStyle/mapStyle.json");
      this.bmap = new BMap.Map("baidu_map");
      let point = new BMap.Point(this.point[0], this.point[1]);
      this.bmap.centerAndZoom(point, this.level);
      this.bmap.setMapStyleV2({ styleJson: styleJson });
      this.bmap.enableScrollWheelZoom();
      this.bmap.disableDoubleClickZoom();
      // 实时显示监听地图级别
      this.bmap.addEventListener("zoomend", e => {
        var ZoomNum = this.bmap.getZoom();
        this.$emit("zoomChange", ZoomNum);
      });
    },
    // 创建点位
    createMarker(img, imgSize, point) {
      var mapPoint = new BMap.Point(point[0], point[1]);
      var imgIcon = new BMap.Icon(img, new BMap.Size(imgSize[0], imgSize[1]));
      return new BMap.Marker(mapPoint, { icon: imgIcon });
      // this.bmap.addOverlay(marker);
    },
    // 添加标注点的点击事件并投放到地图
    addMarker(marker, clickfun, dblclickfun) {
      marker.addEventListener("click", clickfun);
      marker.addEventListener("rightclick", dblclickfun);
      // console.log(this.imgIcon);
      this.bmap.addOverlay(marker);
    },
    // 设置图标对象
    setIconSize(url, size, marker) {
      var imgIcon = new BMap.Icon(url, new BMap.Size(size[0], size[1]));
      var icon = { icon: imgIcon };
      marker.setIcon(icon);
      // console.log(this.imgIcon);
      // debugger
      //  var  size = new BMap.Size(imgSize[0], imgSize[1]);
      //  var offset = {offset: size}
      //  console.log(this.imgIcon.setSize);


      // return this.imgIcon.setSize(offset)
    },
    //添加点位文本标签
    addLabel(content, point, marker, style) {
      if (!style) style = {};
      var opts = {
        position: point,
        offset: new BMap.Size(-10, -10)
      };
      let label = new BMap.Label(content, opts);
      //把label设置到maker上
      marker.setLabel(label);
      label.setStyle(style);
      return label;
    },
    // 清除所有标注点位
    clearOverlays() {
      this.bmap.clearOverlays();
    },
    // 清除特定的点位
    removeOverlay(target) {
      this.bmap.removeOverlay(target);
    },
    // 设置中心点
    setCenter(point,level) {
      // debugger
      let zoomLevel = level||17;
      // if(level){
      //   zoomLevel  =level;
      // }
      let points = new BMap.Point(point[0], point[1]);
      this.bmap.centerAndZoom(points, zoomLevel);
    },
    // 设置缩放级别
     setZoom(level) {
      let points = new BMap.Point(this.point[0], this.point[1]);
      this.bmap.centerAndZoom(points, level);
    },
    // 实时显示层级
    getZoom() {
      return this.bmap.getZoom();
    },
    // 鼠标绘制
    inquireType() {
      var styleOptions = {
        strokeColor: "#3ea5f9", //边线颜色。
        fillColor: "", //填充颜色。当参数为空时，圆形将没有填充效果。
        strokeWeight: 2, //边线的宽度，以像素为单位。
        strokeOpacity: 0.5, //边线透明度，取值范围0 - 1。
        fillOpacity: 0.6, //填充的透明度，取值范围0 - 1。
        strokeStyle: "solid" //边线的样式，solid或dashed。
      };
      //实例化鼠标绘制工具
      var drawingManager = new BMapLib.DrawingManager(this.bmap, {
        isOpen: false, //是否开启绘制模式
        enableDrawingTool: false, //是否显示工具栏
        drawingToolOptions: {
          anchor: BMAP_ANCHOR_TOP_RIGHT, //位置
          offset: new BMap.Size(5, 5) //偏离值
        },
        circleOptions: styleOptions, //圆的样式
        polylineOptions: styleOptions, //线的样式
        polygonOptions: styleOptions, //多边形的样式
        rectangleOptions: styleOptions //矩形的样式
      });
      //添加鼠标绘制工具监听事件，用于获取绘制结果
      let overlaysKC = [];
      var overlaycomplete = function(e) {
        overlaysKC.push(e.overlay);
      };
      drawingManager.addEventListener("overlaycomplete", overlaycomplete);
      // drawingManager.close();
      return { obj: drawingManager, data: overlaysKC };
    },
    closeInquireType(target) {
      target.close();
    },
    // 热力图
    heatMap() {
      // "visible":true, "opacity":70
      var heatmapOverlay = new BMapLib.HeatmapOverlay({ radius: 100 });
      this.bmap.addOverlay(heatmapOverlay);
      return heatmapOverlay;
    },
    // 点聚合
    MarkerClusterer(markers) {
      // var myStyles = [
      //   {
      //     url: require("../../assets/newImage/执法对象.png"),
      //     size: new BMap.Size(50, 50),
      //     fontSize: "14px"
      //   }
      // ];
      var markerClusterer = new BMapLib.MarkerClusterer(this.bmap, {
        markers: markers
      });
      // markerClusterer.setStyles(myStyles);
      return markerClusterer;
    },
    // 根据地理位置调整视野
    setViewport(arr) {
      this.bmap.setViewport(arr);
    },
    // 画线
    createPolyline(arr, options) {
      let defaultOption = {
        strokeColor: "#21e1e8",
        strokeWeight: 3,
        strokeOpacity: 1
      };
      let option = Object.assign(defaultOption, options);
      let arrPoint = arr.map(res => new BMap.Point(res.mlng, res.mlat));
      let Polyline = new BMap.Polyline(arrPoint, defaultOption);
      this.bmap.addOverlay(Polyline);
      return Polyline;
    },
  },


  mounted() {
    this.onReady();
  }
};
</script>


<style lang="less" scoped>
.baidu_map {
  width: 100%;
  height: 100%;
}
</style>

```

### vue中是如何检测数组变化的（源码）

使用了函数劫持的方式，重写了数组方法
Vue将data中的数组，进行了原型链重写，指向了自己定义的数组原型方法。
这样当调用数组api时，可以通知依赖更新。如果数组中包含着引用类型，会对数组中的引用类型再次进行监控。
Object.create()，保存原有原型

### 为何vue采用异步渲染

vue是组件级更新，如果不采用异步更新，那么每次更新数据都会对当前组件重新渲染。

为了性能考虑，vue会在本轮数据更新后，再去异步更新视图

### nextTick原理(源码)

##### 原理

nextTick主要是使用了宏任务和微任务，定义了一个异步方法。多次调用nextTick会将方法存入队列中，通过这个异步方法清空当前队列，所以nextTick就是异步方法

在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM

##### 应用

新的dom没有更新但是需要基于这个dom做操作，需要用到这个nextTick这个api

可以帮助我们在created里实现获取页面元素

当项目中你想在改变DOM元素的数据后基于新的dom做点什么，对新DOM一系列的js操作都需要放进Vue.nextTick()的回调函数中



### 何时需要使用beforeDestroy？

可能在当前组件使用了$on方法，需要在组件销毁前解绑
清除自己定义的定时器
解除事件的原生绑定scroll、mousemove…

### vue中v-if和v-show的区别

v-if如果条件不成立，不会渲染当前指令所在节点的dom元素

v-show切换当前dom的显示和隐藏，本质上display:none

### 为什么v-for和v-if不能连用？

v-for会比v-if的优先级高一些，如果连用的话，会把v-if给每个元素都添加一下，会造成性能问题。
如果确实需要判断每一个，可以用计算属性来解决，先用计算属性将满足条件的过滤出来，然后再去循环。

### **v-for中为什么要用key？**

解决vue中diff算法结构相同key相同，导致内容复用的问题，通过key（最好自定义id，不要用索引），明确dom元素，防止重复渲染。key的作用主要是为了高效的更新虚拟DOM

### 描述组件渲染和更新过程

渲染组件时，会通过Vue.extend方法构建子组件的构造函数，并进行实例化，最终手动调用$mount进行挂载。更新组件时会进行patchVnode流程，核心就是diff算法。

### **v-model的实现原理及如何自定义v-model？**

v-model可以看成是value+input方法的语法糖

其核心就是，一方面modal层通过defineProperty来劫持每个属性，一旦监听到变化通过相关的页面元素更新。另一方面通过编译模板文件，为控件的v-model绑定input事件，从而页面输入能实时更新相关data属性值。

### **Vue中事件绑定的原理**

Vue的事件绑定分为两种：一种是原生的事件绑定，一种是组件的事件绑定
原生dom事件绑定采用的是addEventListener
组件的事件绑定采用的是$on方法

### **组件中的data为什么是个函数？**

同一个组件被复用多次，会创建多个实例。这些实例用的是同一个构造函数，如果data是一个对象的话，所有组件共享了同一个对象。为了保证组件的数据独立性，要求每个组件都必须通过data函数返回一个对象作为组件的状态。

##### vue中的v-html会导致哪些问题

可能会导致XXS攻击
v-html会替换掉标签内的子元素

##### vue中相同逻辑如何抽离

Vue.mixin用法给组件每个生命周期、函数都混入一些公共逻辑

##### 为什么要使用异步组件？

如果组件功能多，打包出的结果会变大，可以采用异步组件的方式来加载组件。主要依赖import()这个语法，可以实现文件的分割加载。

##### 谈谈你对keep-alive的理解（一个组件）

keep-alive可以实现组件的缓存，当组件切换时，不会对当前组件卸载
常用的2个属性include、exclude
常用的2个生命周期activated、deactivated

##### Vue中常见性能优化

###### **编码优化**

不要将所有的数据都放到data中，data中的数据都会增加getter、setter，会收集对应的watcher
vue在v-for时给每项元素绑定事件需要用事件代理
SPA页面采用keep-alive缓存组件
拆分组件（提高复用性、增加代码的可维护性，减少不必要的渲染）
v-if当值为false时，内部指令不会执行，具有阻断功能。很多情况下使用v-if替换v-show
for循环设置key值（提高运算性能，更快定位到节点）
Object.freeze冻结数据
合理使用路由懒加载、异步组件
数据持久化的问题，防抖、节流

Vue路由设置成懒加载

图片资源懒加载

###### **Vue加载性能优化**

第三方模块按需导入（babel-plugin-component）
滚动到可视区域动态加载（https://tangbc.github.io/vue-virtual-scroll-list）
图片懒加载（https://github.com/hilongjw/vue-lazyload.git）

###### **用户体验**

app-skeleton骨架屏

app shell app壳

pwa

###### **SEO优化**

预渲染插件prerender-spa-plugin

服务端渲染ssr

###### **打包优化**

使用cdn的方式加载第三方模块

多线程打包happypack

splitChunks抽离公共文件

sourceMap生成

###### 缓存压缩

客户端缓存、服务端缓存

服务端gzip压缩

##### **Vue3.0的改进**

采用了TS来编写

支持composition API

响应式数据原理改成了proxy

diff对比算法更新，只更新vdom绑定了动态数据的部分

##### 像vue-router，vuex他们都是作为vue插件，请说一下他们分别都是如何在vue中生效的？

通过vue的插件系统，用vue.mixin混入到全局，在每个组件的生命周期的某个阶段注入组件实例

##### 请你说一下vue的设计架构

vue2采用的是典型的混入式架构，类似于express和jquery，各部分分模块开发，再通过一个mixin去混入到最终暴露到全局的类上。
简述一个框架的同时，说出他的设计来源、类似的框架。

##### 请说一下你这个项目中做的事情

这个项目主体是一个vue项目，但是因为是pc端，为了seo，我特意做了ssr。然后这个项目有一套我和同事一起做的专门的组件库。在移动端，我们为了搭配app，也做了移动混合方案。像在首页，因为数据巨大，我们采用了一些优化方案。利用本地缓存数据，对小图标进行了base64转码。

##### 一个优秀的Vue团队代码规范是什么样子的

https://mp.weixin.qq.com/s/RL5P8gwYijMXR5F4xsLv6g



##### vite

- 文档: https://v3.cn.vuejs.org/guide/installation.html
- vite 是一个由原生 ESM 驱动的 Web 开发构建工具。在开发环境下基于浏览器原生 ES imports 开发，
- 它做到了***本地快速开发启动***, 在生产环境下基于 Rollup 打包。
  - 快速的冷启动，不需要等待打包操作；
  - 即时的热模块更新，替换性能和模块数量的解耦让更新飞起；
  - 真正的按需编译，不再等待整个应用编译完成，这是一个巨大的改变。

```
npm init vite-app <project-name>
cd <project-name>
npm install
npm run dev
```

##### Composition API(常用部分)

###### setup

新的option, 所有的组合API函数都在此使用, 只在初始化时执行一次

函数如果返回对象, 对象中的属性或方法, 模板中可以直接使用

###### ref

作用: 定义一个数据的响应式

语法: const xxx = ref(initValue)

- 创建一个包含响应式数据的引用(reference)对象
- js中操作数据: xxx.value
- 模板中操作数据: 不需要.value

一般用来定义一个基本类型的响应式数据

######  reactive

- 作用: 定义多个数据的响应式
- const proxy = reactive(obj): 接收一个普通对象然后返回该普通对象的响应式代理器对象
- 响应式转换是“深层的”：会影响对象内部所有嵌套的属性
- 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象，内部数据都是响应式的

##### proxy的响应式原理

```html
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Proxy 与 Reflect</title>
</head>
<body>
  <script>
    
    const user = {
      name: "John",
      age: 12
    };

    /* 
    proxyUser是代理对象, user是被代理对象
    后面所有的操作都是通过代理对象来操作被代理对象内部属性
    */
    const proxyUser = new Proxy(user, {

      get(target, prop) {
        console.log('劫持get()', prop)
        return Reflect.get(target, prop)
      },

      set(target, prop, val) {
        console.log('劫持set()', prop, val)
        return Reflect.set(target, prop, val); // (2)
      },

      deleteProperty (target, prop) {
        console.log('劫持delete属性', prop)
        return Reflect.deleteProperty(target, prop)
      }
    });
    // 读取属性值
    console.log(proxyUser===user)
    console.log(proxyUser.name, proxyUser.age)
    // 设置属性值
    proxyUser.name = 'bob'
    proxyUser.age = 13
    console.log(user)
    // 添加属性
    proxyUser.sex = '男'
    console.log(user)
    // 删除属性
    delete proxyUser.sex
    console.log(user)
  </script>
</body>
</html>
```

##### setup的一些细节

###### setup执行的时机

- 在beforeCreate之前执行(一次), 此时组件对象还没有创建
- this是undefined, 不能通过this来访问data/computed/methods / props
- 其实所有的composition API相关回调函数中也都不可以

###### setup的返回值

- 一般都返回一个对象: 为模板提供数据, 也就是模板中可以直接使用此对象中的所有属性/方法
- 返回对象中的属性会与data函数返回对象的属性合并成为组件对象的属性
- 返回对象中的方法会与methods中的方法合并成为组件对象的方法
- 如果有重名, setup优先
- 注意:
- 一般不要混合使用: methods中可以访问setup提供的属性和方法, 但在setup方法中不能访问data和methods
- setup不能是一个async函数: 因为返回值不再是return的对象, 而是promise, 模板看不到return对象中的属性数据

###### setup的参数

- setup(props, context) / setup(props, {attrs, slots, emit})
- props: 包含props配置声明且传入了的所有属性的对象
- attrs: 包含没有在props配置中声明的属性的对象, 相当于 this.$attrs
- slots: 包含所有传入的插槽内容的对象, 相当于 this.$slots
- emit: 用来分发自定义事件的函数, 相当于 this.$emit

##### reactive与ref-细节

- 是Vue3的 composition API中2个最重要的响应式API
- ref用来处理基本类型数据, reactive用来处理对象(递归深度响应式)
- 如果用ref对象/数组, 内部会自动将对象/数组转换为reactive的代理对象
- ref内部: 通过给value属性添加getter/setter来实现对数据的劫持
- reactive内部: 通过使用Proxy来实现对对象内部所有数据的劫持, 并通过Reflect操作对象内部数据
- ref的数据操作: 在js中要.value, 在模板中不需要(内部解析模板时会自动添加.value)

##### 计算属性与监视

- computed函数:
  - 与computed配置功能一致
  - 只有getter
  - 有getter和setter
- watch函数
  - 与watch配置功能一致
  - 监视指定的一个或多个响应式数据, 一旦数据变化, 就自动执行监视回调
  - 默认初始时不执行回调, 但可以通过配置immediate为true, 来指定初始时立即执行第一次
  - 通过配置deep为true, 来指定深度监视
- watchEffect函数
  - 不用直接指定要监视的数据, 回调函数中使用的哪些响应式数据就监视哪些响应式数据
  - 默认初始时就会执行第一次, 从而可以收集需要监视的数据
  - 监视数据发生变化时回调

##### 生命周期的对比

vue3的比vue2的快一步执行

- `beforeCreate` -> 使用 `setup()`
- `created` -> 使用 `setup()`
- `beforeMount` -> `onBeforeMount`
- `mounted` -> `onMounted`
- `beforeUpdate` -> `onBeforeUpdate`
- `updated` -> `onUpdated`
- `beforeDestroy` -> `onBeforeUnmount`
- `destroyed` -> `onUnmounted`
- `errorCaptured` -> `onErrorCaptured`

组合式 API 还提供了以下调试钩子函数：

- onRenderTracked
- onRenderTriggered

##### 自定义hook函数

- 使用Vue3的组合API封装的可复用的功能函数
- 自定义hook的作用类似于vue2中的mixin技术
- 自定义Hook的优势: 很清楚复用功能代码的来源, 更清楚易懂

```js
import { ref } from 'vue'
import axios from 'axios'

/* 
使用axios发送异步ajax请求
*/
export default function useUrlLoader<T>(url: string) {

  const result = ref<T | null>(null)
  const loading = ref(true)
  const errorMsg = ref(null)

  axios.get(url)
    .then(response => {
      loading.value = false
      result.value = response.data
    })
    .catch(e => {
      loading.value = false
      errorMsg.value = e.message || '未知错误'
    })

  return {
    loading,
    result,
    errorMsg,
  }
}
```



##### toRefs

把一个响应式对象转换成普通对象，该普通对象的每个 property 都是一个 ref

应用: 当从合成函数返回响应式对象时，toRefs 非常有用，这样消费组件就可以在不丢失响应式的情况下对返回的对象进行分解使用

问题: reactive 对象取出的所有属性值都是非响应式的

解决: 利用 toRefs 可以将一个响应式 reactive 对象的所有原始属性转换为响应式的 ref 属性

```js
<script lang="ts">
import { reactive, toRefs } from 'vue'
/*
toRefs:
  将响应式对象中所有属性包装为ref对象, 并返回包含这些ref对象的普通对象
  应用: 当从合成函数返回响应式对象时，toRefs 非常有用，
        这样消费组件就可以在不丢失响应式的情况下对返回的对象进行分解使用
*/
export default {

  setup () {
	
    const state = reactive({
      foo: 'a',
      bar: 'b',
    })

    const stateAsRefs = toRefs(state)
    // const {foo,bar}=toRefs(state)

    const {foo2, bar2} = useReatureX()

    return {
        //foo,
        //bar
      ...stateAsRefs,
      foo2, 
      bar2
    }
  },
}

function useReatureX() {
  const state = reactive({
    foo2: 'a',
    bar2: 'b',
  })

  setTimeout(() => {
    state.foo2 += '++'
    state.bar2 += '++'
  }, 2000);

  return toRefs(state)
}

</script>
```

##### ref获取元素

利用ref函数获取组件中的标签元素

功能需求: 让输入框自动获取焦点

```js
<template>
  <h2>App</h2>
  <input type="text">---
  <input type="text" ref="inputRef">
</template>

<script lang="ts">
import { onMounted, ref } from 'vue'
/* 
ref获取元素: 利用ref函数获取组件中的标签元素
功能需求: 让输入框自动获取焦点
*/
export default {
  setup() {
    const inputRef = ref<HTMLElement|null>(null)

    onMounted(() => {
      inputRef.value && inputRef.value.focus()
    })

    return {
      inputRef
    }
  },
}
</script>
```

#### Composition API(其它部分)

##### shallowReactive 与 shallowRef

- shallowReactive : 只处理了对象内最外层属性的响应式(也就是浅响应式)
- shallowRef: 只处理了value的响应式, 不进行对象的reactive处理
- 什么时候用浅响应式呢?
  - 一般情况下使用ref和reactive即可
  - 如果有一个对象数据, 结构比较深, 但变化时只是外层属性变化 ===> shallowReactive
  - 如果有一个对象数据, 后面会产生新的对象来替换 ===> shallowRef

#####  readonly 与 shallowReadonly

- readonly:
  - 深度只读数据
  - 获取一个对象 (响应式或纯对象) 或 ref 并返回原始代理的只读代理。
  - 只读代理是深层的：访问的任何嵌套 property 也是只读的。
- shallowReadonly
  - 浅只读数据
  - 创建一个代理，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换
- 应用场景:
  - 在某些特定情况下, 我们可能不希望对数据进行更新的操作, 那就可以包装生成一个只读代理对象来读取数据, 而不能修改或删除

##### toRaw 与 markRaw

- toRaw
  - 返回由 `reactive` 或 `readonly` 方法转换成响应式代理的普通对象。
  - 这是一个还原方法，可用于临时读取，访问不会被代理/跟踪，写入时也不会触发界面更新。
- markRaw
  - 标记一个对象，使其永远不会转换为代理。返回对象本身
  - 应用场景:
    - 有些值不应被设置为响应式的，例如复杂的第三方类实例或 Vue 组件对象。
    - 当渲染具有不可变数据源的大列表时，跳过代理转换可以提高性能。

##### toRef

- 为源响应式对象上的某个属性创建一个 ref对象, 二者内部操作的是同一个数据值, 更新时二者是同步的
- 区别ref: 拷贝了一份新的数据值单独操作, 更新时相互不影响
- 应用: 当要将 某个prop 的 ref 传递给复合函数时，toRef 很有用

##### provide 与 inject

- provide`和`inject`提供依赖注入，功能类似 2.x 的`provide/inject
- 实现跨层级组件(祖孙)间通信