## React入门

##### 学习react之前要掌握的javascript基础知识

```json
判断this指向    class（类）   es6语法规范   npm包管理器
原型 原型链   数组常用方法   模块化
```

##### react的特点

```js
采用组件化模式，声明式编码，提高开发效率以及组件复用率
在ReactNative中可以采用react语法进行原生移动端开发
使用虚拟dom+优秀的diff算法，尽量减少与真实dom的交互
```

##### react高效的原因

```
使用虚拟dom，不总是直接操作页面的真实dom
diff算法，最小化页面重绘
```

##### 相关的js库

```
react.js       react核心库
react-dom.js   提供操作dom的react扩展库
babel.min.js    解析jsx语法代码转化为js代码的库 
```

##### react创建虚拟dom的两种方式

```js
jsx的语法
let vdom = <h1>Hello,,,React</h1>;
原生js的语法
let vdom = React.createElement('h1',{id:'title'},React.createElement('span',{},'helloReact'));
//渲染到页面上
ReactDOM.render(vdom,document.getElementById('app'))
```

##### react组件的两种方式

###### 函数式组件

```json
适用于简单组件的定义（无状态，没有this，没有实例）
  函数式组件的this指向式undefined，因为babel编译后开启了严格模式
函数式组件渲染的过程
  1.React解析组件标签，找到了该函数组件。
  2.发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DoM，随后呈现在页面中。
```

###### 类式组件

```
适用于复杂组件的定义（有状态，有this(指向实例对象)，有实例）
	类组件中this的指向是组件的实例对象，react在执行过程中会自动帮我们生成一个实例
类组件渲染的过程
	1.React解析组件标签，找到了MyComponent组件。
	2.发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。
```

##### 改变react类组件中类方法的this指向的两种方法

```
由于javascript类中的方法自动开启严格模式，所以类方法的this指向的是undefined
所以类组件中的自定义方法的this都是undefined，需要手动改变this指向到当前的组件实例上
 1 在类构造器中使用bind方法改变this指向并重新赋值
 2 在类中定义方法时，使用箭头函数统一this指向
```

##### setState的两种形式

```
对象式的setState
	const{count}=this.state
	this.setstate（{count:count+1}）
函数式的setState 
	this.setstate（(state,props)=>（{count:state.count+1}））
```

##### react组件的三大核心属性

```json
state：state是组件最重要的属性，是一个对象；同时state是一个状态机，通过更新state来更新页面
props：用于组件传值，是一个只读属性。每个组件都有一个props属性，保存着组件标签的所有属性
ref：（不要过度使用）
	字符串形式的ref(存在效率问题，已过时) 
	回调函数形式的ref(用函数接收参数重新赋值，分为内联和类绑定式) 
	使用createRef这个api创建
```

#####     几种ref的写法

```jsx
1 ref="input" this.refs.input
2 ref={(node)=>{this.input=node}}  或 ref={this.input}  input=()=>{}
3 ref={this.myref}  myref = React.createRef()
```

##### react中的事件处理

```
1 指定的事件需要驼峰大小写
2 react使用的是自定义事件，不是dom原生事件----为了更好的兼容性
3 react中的事件是用过事件委托的方式处理的(委托给最外层的元素)----为了更高效
4 事件本身会像原生事件一个传递一个事件源参数，不需要过度使用ref属性
5 react事件的使用，本身是给事件传递一个回调函数。所以当需要传参时，需要在函数体返回一个函调函数(高阶函数)或者直接在事件后面写一个回调函数再调用定义好的函数
```

##### 受控组件和非受控组件的区别

```
受控组件
在React中，每当表单的状态发生变化时，都会被写入到组件的state中，这种组件在React被称为受控组件。
非受控组件
在React中，非受控组件是一种反模式，它的值不受组件自身的state或者props控制，通常需要为其添加ref prop来访问渲染后的底层DOM元素。
```

##### react表单提交高级写法(高阶函数和函数柯里化)

```js
saveFormData=(dataType)=>{
return(event)=>{
  this.setstate({[dataType]:event.target.value})
 }
}
<input onChange={ this. saveFormData(' username')}/>
<input onChange={ this. saveFormData(' password')}/>
```

##### react生命周期（旧）

```jsx
1.初始化阶段：由ReactDoM.render（）触发---初次渲染
	1.constructor（）
	2.componentWi1lMount（）
	3.render（）
	4.componentDidMount（）
2.更新阶段：由组件内部this.setsate（）或父组件render触发
	1.shouldComponentUpdate（）
	2.componentwi11Update（）
	3.render（）
	4.componentDidupdate（）
3.卸载组件：由ReactDOM.unmountComponentAtNode（）触发
	1.componentWi11Unmount（）
```

##### react生命周期（新）

```
1.初始化阶段：由ReactDoM.render（）触发---初次渲染
	1.constructor（）
	2.getDerivedStateFromProps
	3.render（）
	4.componentDidMount（）=====>常用一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请		求、订阅消息
2.更新阶段：由组件内部this.setsate（）或父组件重新render触发
	1.getDerivedStateFromProps
	2.shouldComponentUpdate（）
	3.render（）
	4.getsnapshotBeforeUpdate
	5.componentDidUpdate（）
3.卸载组件：由ReactDOM.unmountComponentAtNode（）触发
	1.componentwi11Unmount（）=====>常用一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消		息
```

##### react样式的模块化

```
1 把css文件后缀改成index.module.css
  引入的时候：import hello from 'index.module.css'，生成一个对象  
  使用：hello.类名
2 使用预处理器嵌套，在最外层套一个不一样的类型即可
```

##### react组件传值

```
父传子：在父组件的子组件标签上传递一个值xxx，子组件通过通过props接收(this.props.xxx)
子传父：在父组件定义一个函数func，再传递给子组件；需要传值的时候子组件通过props调用该函数					(this.props.func())
兄弟组件(任意组件)：发布者-订阅者模式(消息订阅模式)，引入pubsub.js这个库
```

##### todoList案例相关知识点

```json
1.拆分组件、实现静态组件，注意：className、style的写法
2.动态初始化列表，如何确定将数据放在哪个组件的state中？
	某个组件使用：放在其自身的state中
	某些组件使用：放在他们共同的父组件state中（官方称此操作为：状态提升）
3.关于父子之间通信：
	1.【父组件】给【子组件】传递数据：通过props传递
	2.【子组件】给【父组件】传递数据：通过props传递，要求父提前给子传递一个函数
4.注意defaultchecked和checked的区别，类似的还有：defaultValue和 value
5.状态在哪里，操作状态的方法就在哪里
```

##### context

```
一种组件间通信方式，常用于祖组件与后代组件间的通信
```

##### component的两个问题（组件优化）

```
1.只要执行setState（），即使不改变状态数据，组件也会重新render（）==>效率低
2.只当前组件重新render（），就会自动重新render子组件，纵使子组件没有用到父组件的任何数据==>效率低
优化：只有当组件的state或props数据发生改变时才重新render()
原因：Component中的shouldComponentUpdate()总是返回true
解决：
	1 手动在shouldComponentUpdate里面进行state或者props的判断
	2 使用pureComponent替代Component
```

##### prueComponent

```json
Pur eComponent重写了shouldcomponentUpdate（），只有state或props数据有变化才返回true
注意：
	只是进行state和props数据的浅比较，如果只是数据对象内部数据变了，返回false
	不要直接修改state数据，而是要产生新数据
```

##### 如何向组件内部动态传入带内容的结构=====插槽

``` jsx
Vue中：
	使用slot技术，也就是通过组件标签体传入结构 <A> <B/> </A>
React中：
	使用children props：通过组件标签体传入结构
	使用render props：通过组件标签属性传入结构，一般用render函数属性  
```

##### children props

```jsx
<A>
<B>xxxx</B>
</A>
{this.props.children}
问题：如果B组件需要A组件内的数据，==>做不到
```

##### render props

```jsx
<A render={（data）=><C data={data}></C>}></A>
A组件：{this.props.render（内部state数据）}
c组件：读取A组件传入的数据显示{this.props.data}
```

##### 组件通信方式总结

```
1.props：
	（1）.children props
	（2）.render props
2.消息订阅-发布：
	pubs-sub、eent等等
3.集中式管理：
	redux、dva等等
4.conText：
	生产者-消费者模式，借助provider
```

## react路由

##### react路由(路由组件与一般组件)

```jsx
直接在app文件引入路由库，在app组件包裹一个<browerRouter>或<hashRouter>

1.写法不同：
	一般组件：<Demo/>
	路由组件：<Route path="/demo"component={Demo}/>
2.存放位置不同：
	一般组件：components
	路由组件：pages
3.接收到的props不同：
	一般组件：写组件标签时传递了什么，就能收到什么
	路由组件：接收到三个固定的属性(history,location,match)
```

##### BrowserRouter与HashRouter的区别

```json
1.底层原理不一样：
	BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。
	HashRouter使用的是URL的哈希值。
2.path表现形式不一样
	BrowserRouter的路径中没有#，例如：localhost：300e/demo/test HashRouter的路径包含#，例如：		1ocalhost：3000/#/demo/test
3.刷新后对路由state参数的影响
	（1）.BrowserRouter没有任何影响，因为state保存在history对象中。
	（2）.HashRouter刷新后会导致路由state参数的丢失！！！
4.备注：HashRouter可以用于解决一些路径错误相关的问题。
```

##### react路由的NavLink与封装NavLink

```js
1.NavLink可以实现路由链接的高亮，通过activeClassName指定样式名
2.标签体内容是一个特殊的标签属性
3.通过this.props.children可以获取标签体内容
<NaVLink  to="/about" children="About"/>
<NavLink activeClassName="atguigu" {...this.propsp}/>
```

##### react路由传参

```jsx
1.params参数
	路由链接（携带参数）：<Link to='/demo/test/tom/18'}>详情</Link>
	注册路由（声明接收）：<Route path="/demo/test/：name/：age"component={Test}/>
	接收参数：this.props.match.params
2.search参数
	路由链接（携带参数）：<Link to='/demo/test？name=tom&age=18'}>详情</Link>
	注册路由（无需声明，正常注册即可）：<Route path="/demo/test"component={Test}/>
	接收参数：this.props.location.search备注：获取到的search是urlencoded编码字符串，需要借助		querystring解析
3.state参数
	路由链接（携带参数）：<Link to={{path:'/demo/test'，state:{name:'tom'，age：18}}}>详情		</Link>
	注册路由（无需声明，正常注册即可）：<Route path="/demo/test"component={Test}/>
	接收参数：this.props.location.state备注：刷新也可以保留住参数
```

##### 编程时路由导航

```
push跳转+携带params参数
	this.props.history.push（"/home/message/detail/${id}/${title}）
push跳转+携带search参数
	this.props.history.push（"/home/message/detai1？id=${id）&title=${title）
push跳转+携带state参数
	this.props.history.push（"/home/message/detai1，{id，title}）
```

##### 一般组件加工成路由组件

```json
import{withRouter}from'react-router-dom'
class Header extends Component{}
export default withRouter（Header）
//withRouter可以加工一般组件，让一般组件具备路由组件所特有的API
//withRouter的返回值是一个新组件
```

##### 路由懒加载

```jsx
从react中引入lazy，Suspense
用lazy引入路由组件 const About=lazy(()=>import('./About'))
组件使用 
	<Suspense fallback={<h1>Loading......k/h1>}>
	<Route path="/about" component={About}/>
	</Suspense>
```

## redux

##### 什么是redux

```
1.redux是一个专门用于状态管理的JS库（不是react插件库）。
2.它可以用在react，angular，vue等项目中，但基本与react配合使用。
3.作用：集中式管理react应用中多个组件共享数据的状态。	
```

##### redux三大原则

```
（1）单一数据源，一个应用只有一个 store，保存着state
（2）State 是只读的，唯一改变 state 的方法是触发 action
（3）通过纯函数来修改，编写reducer函数来修改state。reducer函数接收前一次的 state 和 action，返回		 新的state
```

##### redux身上挂载的4个方法

```
1 createStore 用于创建仓库的函数，接收两个参数
2 applymiddleWare createStore的第二个参数，配合redux-thunk开启异步action功能。是一个中间件函数，参数是thunk插件
3 combineReducers 合并所有的reducer的函数，参数是一个对象
4 bindActionCreators	合并所有的action
```



##### redux三个核心概念

1 action

```
1.动作的对象
2.包含2个属性
	type：标识属性，值为字符串，唯一，必要属性
	data：数据属性，值类型任意，可选属性
3.例子：{type:'ADD_STUDENT，data:{name:'tom'，age：18}}
```

2 reducer

```
reducer是一个函数
1.用于初始化状态、加工状态。
2.加工时，根据旧的state和action，产生新的state的纯函数，返回新的状态。
```

3 store

```
1.将state、action、reducer 联系在一起的对象
2.此对象的功能？(store上挂载的三个方法)
1）getState()：得到state
2）dispatch（action）：分发action，触发reducer 调用，产生新的 statee
3）subscribe（listener）：注册监听，当产生了新的state时，自动调用
```

##### redux基本流程

```
用户通过界面组件 触发ActionCreator，携带Store中的旧State与Action 流向Reducer,Reducer返回新的state，并更新界面
```

1 创建一个仓库store

```js
import {createStore,applymiddleWare,combineReducers} from 'redux'
//引入redux-thunk，用于支持异步action
import thunk from 'redux-thunk'
//这里传入每一个reducer，最后用combineReducers一起传入给store
import countReducer form './reducer'
//combineReducers传入的对象就是总的reducers状态对象
const allreducers = combineReducers({reducer：countReducer})
const store = createStore(allreducers,applymiddleWare(thunk));
export default store
```

1.2 定义一个常量模块给reducer文件和action文件用

```js
该模块是用于定义action对象中type类型的常量值
export const INCREMENT='increment'
export const DECREMENTT='decrement|
```

2 创建一个reducer（后续所有reducer操作都在一个reducer文件夹里）

```js
import { INCEMENT, DECREMENT} from ./constant'
export default function countReducer(preState=initState，action）{
const {type,data} = action;
switch（type）{
	case INCEMENT：
	  return preState + datacase
	case DECREMENT：
	  return preState-data 
 	default:return preState
  }
}

```

3 创建一个action文件，用于专门生成action对象(action同步是对象，异步是函数)

（后续所有action操作都在一个action文件夹里）

```jsx
import { INCREMENT, DECREMENT} from'./constant'
export const createIncrementAction=data=>({type:INCREMENT,data})
export const createDecrementAction=data=>({type:DECREMENT,data})
//异步action，就是指action的值为函数，异步action中一般都会调用同步action 
export const  createIncrementAsyncAction = (data, time) => {
        return (dispatch) => {
            setTimeout(() => {
                dispatch(createIncrementAction(data))
            }, time)
        }
    }
```

4 组件获取store里面保存的状态并且派发事件改变状态

```jsx
引入store仓库
import store from'../../redux/store'
引入actionCreator，专门用于创建action对象
import{createIncrementAction}from'../../redux/count_action'
组件中展示store中需要的state
{store.getState()}
事件派发改变状态
store.dispatch(createIncrementAction(value))
```

5 监听状态的变化，驱动视图更新

```jsx
componentDidMount（）{
	render store.subscribe（（）=>{
	  this.setState（{}）
  }）
}

优化（在根节点就进行检测，不需要每个组件都调用监测api）
store.subscribs(()=>{
 ReactDoM.render(<App/>,document.getElementById('root'))
})
```

#### react-redux

##### react-redux的相关描述

```
1.所有的UI组件都应该包裹一个容器组件，他们是父子关系。
2.容器组件是真正和redux打交道的，里面可以随意的使用redux的api。
3.UI组件中不能使用任何redux的api。
4.容器组件会传给U组件：（1）.redux中所保存的状态。（2）.用于操作状态的方法。
5.备注：容器给UI传递：状态、操作状态的方法，均通过props传递。
```

##### react-redux的基本流程

1 创建一个 UI组件,用于连接UI组件和redux（核心）

```js
//引入actionCreator，专门用于创建action对象
import{createIncrementAction}from'../../redux/count_action'
//映射状态(代码需要简写优化)
function mapStateToProps(state) {
        return { count: state }
    }
//映射操作状态的方法（写法1函数形式）(代码需要简写优化)
function mapDispatchToprops(dispatch) {
        return {
            jia: number => dispatch(createIncrementAction(number))
        }
    }

//映射操作状态的方法（精简写法2对象形式，这是因为对象的值就是调用了函数）
let mapDispatchToprops={jia:createIncrementAction}

//使用connect（）（）创建并暴露一个Count的容器组件
export default connect（mapStateToprops，mapDispatchToProps）（CountUI）
```

1.2 在组件中传入store仓库

```js
<Count store={store}/>
    
优化：这里不需要每个组件都传入store，可以使用Provider在根节点传入，给每个组件使用
import{Provider} from 'react-redux'
ReactDOM.render(
        <Provider store={store}>
            <App/>
        </Provider>, 
        document.getElementById(' root')
        )
```

2 UI组件中使用容器组件的状态和方法

```
UI组件此时相当于子组件，一切的状态和方法，都可以通过props调用
```

3 检测状态的变化，驱动视图的更新

```
之前纯redux，需要用store的subscribs方法去检测状态变化更新视图
现在用react-redux搭配redux，不需要手动去检测，react-redux自动帮我们检测了。
原因在于connect的时候就帮我们做了对应的操作，把ui组件和容器组件链接起来
```



##  hooks

##### 什么是hooks

```
（1）.Hook是React 16.8.05本增加的新语法
（2）.可以让你在函数组件中使用state以及其他的React 特性
```

##### 三个常用的Hook

```
（1）State Hook：React.useState（）
（2）Effect Hook:React.useEffect（）
（3）Ref Hook:React.useRef（）
```

##### State Hook

```
（1）state Hook让函数组件也可以有state状态，并进行状态数据的读写操作
（2）语法：const[count,setCount]=React.usestate（initValue）
（3）usestate()说明：
	参数：第一次初始化指定的值在内部作缓存
	返回值：包含2个元素的数组，第1个为内部当前状态值，第2个为更新状态值的函数
（4）setCount（）2种写法：
	setCount（newvalue）:参数为非函数值，直接指定新的状态值，内部用其覆盖原来的状态值
	setCount（value =>newvalue）:参数为函数，接收原本的状态值，返回新的状态值，内部用其覆盖原来的状态	 值
```

##### Effect Hook

```json
（1）.Effect Hook 可以让你在函数组住中执行副作用操作（用于模拟类组件中的生命周期钩子）
（2）.React中的副作用操作：
	发ajax请求数据获取
	设置订阅/启动定时器
	手动更改真实DOM
（3）.语法和说明：
	useEffect(()=>{
	//在此可以执行任何带副作用操作
	return（）=>{  //在组件卸载前执行
	//在此做一些收尾工作，比如清除定时器/取消订阅等
	}}，[statevalue]）//如果指定的是[]，回调函数只会在第一次render（）后执行
（4）.可以把useEffect Hook 看做如下三个函数的组合
	componentDidMount()
	componentDidupdate（）
	componentwi11unmount（）
```

##### ref hook

```
（1）.Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据
（2）.语法：const refcontainer=useRef（）
（3）.作用：保存标签对象，功能与React.createRef（）一样
```

## react脚手架配置网络代理总结

##### 在package.json中追加如下配置

```
"proxy":"http://localhost:5000"
说明：
	1. 优点：配置简单，前端请求资源时可以不加任何前缀。
	2. 缺点：不能配置多个代理。
	3. 工作方式：上述方式配置代理，当请求了3000不存在的资源时，那么该请求会转发给5000 （优先匹配前端资源）
```

##### 创建代理配置文件(src/setupProxy.js)

```js
const proxy = require('http-proxy-middleware')

module.exports = function(app) {
  app.use(
    proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
      target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
      changeOrigin: true, //控制服务器接收到的请求头中host字段的值
      /*
      	changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
      	changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
      	changeOrigin默认值为false，但我们一般将changeOrigin值设为true
      */
      pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
    }),
    proxy('/api2', { 
      target: 'http://localhost:5001',
      changeOrigin: true,
      pathRewrite: {'^/api2': ''}
    })
  )
}

说明：
1. 优点：可以配置多个代理，可以灵活的控制请求是否走代理。
2. 缺点：配置繁琐，前端请求资源时必须加前缀。
```

