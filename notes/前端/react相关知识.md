## React入门

##### 学习react之前要掌握的javascript基础知识

判断this指向    class（类）   es6语法规范   npm包管理器
原型 原型链   数组常用方法   模块化

##### react的特点

采用组件化模式，声明式编码，提高开发效率以及组件复用率
在ReactNative中可以采用react语法进行原生移动端开发
使用虚拟dom+优秀的diff算法，尽量减少与真实dom的交互

##### react高效的原因

使用虚拟dom，不总是直接操作页面的真实dom
diff算法，最小化页面重绘

##### 相关的js库

react.js       react核心库
react-dom.js   提供操作dom的react扩展库
babel.min.js    解析jsx语法代码转化为js代码的库 

##### react创建虚拟dom的两种方式

jsx的语法

```js
let vdom = <h1>Hello React</h1>;
```

原生js的语法

```js
let vdom = React.createElement('h1',{id:'title'},React.createElement('span',{},'helloReact'));
```

渲染到页面上

```js
ReactDOM.render(vdom,document.getElementById('app'))
```

##### react组件的两种方式

###### 函数式组件

适用于简单组件的定义（无状态，没有this，没有实例）
 函数式组件的this指向式undefined，因为babel编译后开启了严格模式
函数式组件渲染的过程
  1.React解析组件标签，找到了该函数组件。
  2.发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DoM，随后呈现在页面中。

###### 类式组件

适用于复杂组件的定义（有状态，有this(指向实例对象)，有实例）
	类组件中this的指向是组件的实例对象，react在执行过程中会自动帮我们生成一个实例
类组件渲染的过程
	1.React解析组件标签，找到了MyComponent组件。
	2.发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。

##### 改变react类组件中类方法的this指向的两种方法

由于javascript类中的方法自动开启严格模式，所以类方法的this指向的是undefined
所以类组件中的自定义方法的this都是undefined，需要手动改变this指向到当前的组件实例上
 1 在类构造器中使用bind方法改变this指向并重新赋值
 2 在类中定义方法时，使用箭头函数统一this指向

##### setState的两种形式

对象式的setState

```js
const{count}=this.state
	this.setstate（{count:count+1}）
```

函数式的setState 

```js
this.setstate（(state,props)=>（{count:state.count+1}））
```



##### react组件的三大核心属性

###### state

state是组件最重要的属性，是一个对象；同时state是一个状态机，通过更新state来更新页面

###### props

用于组件传值，是一个只读属性。每个组件都有一个props属性，保存着组件标签的所有属性

###### ref（不要过度使用）

​	字符串形式的ref(存在效率问题，已过时) 
​	回调函数形式的ref(用函数接收参数重新赋值，分为内联和类绑定式) 
​	使用createRef这个api创建

#####     几种ref的写法

```jsx
1 ref="input" this.refs.input
2 ref={(node)=>{this.input=node}}  或 ref={this.input}  input=()=>{}
3 ref={this.myref}  myref = React.createRef()
```

##### react中的事件处理

1 指定的事件需要驼峰大小写
2 react使用的是自定义事件，不是dom原生事件----为了更好的兼容性
3 react中的事件是用过事件委托的方式处理的(委托给最外层的元素)----为了更高效
4 事件本身会像原生事件一个传递一个事件源参数，不需要过度使用ref属性
5 react事件的使用，本身是给事件传递一个回调函数。所以当需要传参时，需要在函数体返回一个函调函数(高阶函数)或者直接在事件后面写一个回调函数再调用定义好的函数

##### 受控组件和非受控组件的区别

###### 受控组件

在React中，每当表单的状态发生变化时，都会被写入到组件的state中，这种组件在React被称为受控组件。

###### 非受控组件

在React中，非受控组件是一种反模式，它的值不受组件自身的state或者props控制，通常需要为其添加ref prop来访问渲染后的底层DOM元素。

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

##### react生命周期（新）

1.初始化阶段：由ReactDoM.render（）触发---初次渲染
	1.constructor（）
	2.getDerivedStateFromProps
	3.render（）
	4.componentDidMount（）=====>常用一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息
2.更新阶段：由组件内部this.setsate（）或父组件重新render触发
	1.getDerivedStateFromProps
	2.shouldComponentUpdate（）
	3.render（）
	4.getsnapshotBeforeUpdate
	5.componentDidUpdate（）
3.卸载组件：由ReactDOM.unmountComponentAtNode（）触发
	1.componentwi11Unmount（）=====>常用一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息

##### react样式的模块化

1 把css文件后缀改成index.module.css
  引入的时候：import hello from 'index.module.css'，生成一个对象  
  使用：hello.类名
2 使用预处理器嵌套，在最外层套一个不一样的类型即可

##### react组件传值

父传子：在父组件的子组件标签上传递一个值xxx，子组件通过通过props接收(this.props.xxx)
子传父：在父组件定义一个函数func，再传递给子组件；需要传值的时候子组件通过props调用该函数					(this.props.func())
兄弟组件(任意组件)：发布者-订阅者模式(消息订阅模式)，引入pubsub.js这个库

##### todoList案例相关知识点

1.拆分组件、实现静态组件，注意：className、style的写法
2.动态初始化列表，如何确定将数据放在哪个组件的state中？
	某个组件使用：放在其自身的state中
	某些组件使用：放在他们共同的父组件state中（官方称此操作为：状态提升）
3.关于父子之间通信：
	1.【父组件】给【子组件】传递数据：通过props传递
	2.【子组件】给【父组件】传递数据：通过props传递，要求父提前给子传递一个函数
4.注意defaultchecked和checked的区别，类似的还有：defaultValue和 value
5.状态在哪里，操作状态的方法就在哪里

##### context

一种组件间通信方式，常用于祖组件与后代组件间的通信

##### component的两个问题（组件优化）

1.只要执行setState（），即使不改变状态数据，组件也会重新render（）==>效率低
2.只当前组件重新render（），就会自动重新render子组件，纵使子组件没有用到父组件的任何数据==>效率低

优化：只有当组件的state或props数据发生改变时才重新render()
原因：Component中的shouldComponentUpdate()总是返回true
解决：
	1 手动在shouldComponentUpdate里面进行state或者props的判断
	2 使用pureComponent替代Component

##### prueComponent

PureComponent重写了shouldcomponentUpdate（），只有state或props数据有变化才返回true
注意：
	只是进行state和props数据的浅比较，如果只是数据对象内部数据变了，返回false
	不要直接修改state数据，而是要产生新数据

##### 如何向组件内部动态传入带内容的结构=====插槽

Vue中：
	使用slot技术，也就是通过组件标签体传入结构 <A> <B/> </A>
React中：
	使用children props：通过组件标签体传入结构
	使用render props：通过组件标签属性传入结构，一般用render函数属性  

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

1.props：
	（1）.children props
	（2）.render props
2.消息订阅-发布：
	pubs-sub、eent等等
3.集中式管理：
	redux、dva等等
4.conText：
	生产者-消费者模式，借助provider

## react路由

##### react路由(路由组件与一般组件)

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

##### BrowserRouter与HashRouter的区别

1.底层原理不一样：
	BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。
	HashRouter使用的是URL的哈希值。
2.path表现形式不一样
	BrowserRouter的路径中没有#，例如：localhost：300e/demo/test HashRouter的路径包含#，例如：		1ocalhost：3000/#/demo/test
3.刷新后对路由state参数的影响
	（1）.BrowserRouter没有任何影响，因为state保存在history对象中。
	（2）.HashRouter刷新后会导致路由state参数的丢失！！！
4.备注：HashRouter可以用于解决一些路径错误相关的问题。

##### react路由的NavLink与封装NavLink

1.NavLink可以实现路由链接的高亮，通过activeClassName指定样式名
2.标签体内容是一个特殊的标签属性
3.通过this.props.children可以获取标签体内容

```js
<NaVLink  to="/about" children="About"/>
<NavLink activeClassName="atguigu" {...this.propsp}/>
```



##### react路由传参

1.params参数
	路由链接（携带参数）：<Link to='/demo/test/tom/18'}>详情</Link>
	注册路由（声明接收）：<Route path="/demo/test/：name/：age"component={Test}/>
	接收参数：this.props.match.params
2.search参数
	路由链接（携带参数）：<Link to='/demo/test？name=tom&age=18'}>详情</Link>
	注册路由（无需声明，正常注册即可）：<Route path="/demo/test"component={Test}/>
	接收参数：this.props.location.search备注：获取到的search是urlencoded编码字符串，需要借助querystring解析
3.state参数
	路由链接（携带参数）：<Link to={{path:'/demo/test'，state:{name:'tom'，age：18}}}>详情</Link>
	注册路由（无需声明，正常注册即可）：<Route path="/demo/test"component={Test}/>
	接收参数：this.props.location.state备注：刷新也可以保留住参数

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

1.redux是一个专门用于状态管理的JS库（不是react插件库）。
2.它可以用在react，angular，vue等项目中，但基本与react配合使用。
3.作用：集中式管理react应用中多个组件共享数据的状态。	

##### redux三大原则

（1）单一数据源，一个应用只有一个 store，保存着state
（2）State 是只读的，唯一改变 state 的方法是触发 action
（3）通过纯函数来修改，编写reducer函数来修改state。reducer函数接收前一次的 state 和 action，返回新的state

##### redux身上挂载的4个方法

1 createStore 用于创建仓库的函数，接收两个参数
2 applymiddleWare createStore的第二个参数，配合redux-thunk开启异步action功能。是一个中间件函数，参数是thunk插件
3 combineReducers 合并所有的reducer的函数，参数是一个对象
4 bindActionCreators合并所有的action

##### redux三个核心概念

######  action

1.动作的对象
2.包含2个属性
	type：标识属性，值为字符串，唯一，必要属性
	data：数据属性，值类型任意，可选属性
3.例子：{type:'ADD_STUDENT，data:{name:'tom'，age：18}}

######  reducer

reducer是一个函数
1.用于初始化状态、加工状态。
2.加工时，根据旧的state和action，产生新的state的纯函数，返回新的状态。

###### store

1.将state、action、reducer 联系在一起的对象
2.此对象的功能？(store上挂载的三个方法)
1）getState()：得到state
2）dispatch（action）：分发action，触发reducer 调用，产生新的 statee
3）subscribe（listener）：注册监听，当产生了新的state时，自动调用

##### redux基本流程

用户通过界面组件 触发ActionCreator，携带Store中的旧State与Action 流向Reducer,Reducer返回新的state，并更新界面

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

1.所有的UI组件都应该包裹一个容器组件，他们是父子关系。
2.容器组件是真正和redux打交道的，里面可以随意的使用redux的api。
3.UI组件中不能使用任何redux的api。
4.容器组件会传给U组件：（1）.redux中所保存的状态。（2）.用于操作状态的方法。
5.备注：容器给UI传递：状态、操作状态的方法，均通过props传递。

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

UI组件此时相当于子组件，一切的状态和方法，都可以通过props调用

3 检测状态的变化，驱动视图的更新

之前纯redux，需要用store的subscribs方法去检测状态变化更新视图
现在用react-redux搭配redux，不需要手动去检测，react-redux自动帮我们检测了。
原因在于connect的时候就帮我们做了对应的操作，把ui组件和容器组件链接起来





##  hooks

##### 纯函数组件的特点

- 纯函数组件**没有状态**
- 纯函数组件**没有生命周期**
- 纯函数组件没有`this`
- 只能是纯函数

这就注定，我们所推崇的函数组件，只能做UI展示的功能，涉及到状态的管理与切换，我们不得不用类组件或者redux

##### 类函数组件的特点

- 大型组件很难拆分和重构，也很难测试。
- 业务逻辑分散在组件的各个方法之中，导致重复逻辑或关联逻辑。
- 组件类引入了复杂的编程模式，比如 render props 和高阶组件。

##### react hooks出现的原因

为了解决这种，*类组件功能齐全却很重，纯函数很轻便却有上文几点重大限制*，React团队设计了**React Hooks**

React Hooks就是加强版的函数组件，我们可以完全不使用 `class`，就能写出一个全功能的组件

##### 什么是hooks

用函数的形式代替原来的继承类的形式，并且使用预函数的形式管理`state`，有Hooks可以不再使用类的形式定义组件了。这时候你的认知也要发生变化了，原来把组件分为有状态组件和无状态组件，有状态组件用类的形式声明，无状态组件用函数的形式声明。那现在所有的组件都可以用函数来声明了，即所有的组件都可以使用函数的形式进行编写

##### useState():状态钩子

在`useState()`中，它接受状态的初始值作为参数，即上例中计数的初始值，它返回一个数组，其中数组第一项为一个变量，指向状态的当前值。类似`this.state`,第二项是一个函数，用来更新状态,类似`setState`。该函数的命名，我们约定为`set`前缀加状态的变量名

第二个参数最好写成参数的形式，因为他会被当前的上下文的环境所影响，拿不到最新的值

```js
const [ count, setCount ] = useState(0)
 setCount((count)=>count+=1)
```

**计数器例子**

```js
import React, {useState} from 'react'
const AddCount = () => {
  const [ count, setCount ] = useState(0)
  const addcount = () => {
    let newCount = count
    setCount(newCount+=1)
  } 
  return (
    <>
      <p>{count}</p>
      <button onClick={addcount}>count++</button>
    </>
  )
}
export default AddCount 
```

##### useContext():共享状态钩子

该钩子的作用是，在组件之间共享状态。关于Context这里不再赘述，其作用就是可以做状态的分发，在React16.X以后支持，避免了react逐层通过Props传递数据

```js
import React,{ useContext } from 'react'
const Ceshi = () => {
  const AppContext = React.createContext({})
  const A =() => {
    const { name } = useContext(AppContext)
    return (
        <p>我是A组件的名字{name}<span>我是A的子组件{name}</span></p>
    )
}
const B =() => {
  const { name } = useContext(AppContext)
  return (
      <p>我是B组件的名字{name}</p>
  )
}
  return (
    <AppContext.Provider value={{name: 'hook测试'}}>
    <A/>
    <B/>
    </AppContext.Provider>
  )
}
export default Ceshi 
```

##### useReducer():Action钩子

在使用React的过程中，如遇到状态管理，我们一般会用到Redux,而React本身是不提供状态管理的。而`useReducer()`为我们提供了状态管理。首先，关于redux我们都知道，其原理是我们通过用户在页面中发起action,从而通过reducer方法来改变state,从而实现页面和状态的通信。而Reducer的形式是`(state, action) => newstate`。类似，我们的`useReducer()`是这样的

```js
const [state, dispatch] = useReducer(reducer, initialState)
```

它接受reducer函数和状态的初始值作为参数，返回一个数组，其中第一项为*当前的*状态值，第二项为发送action的dispatch函数。下面我们依然用来实现一个计数器。
 和redux一样，我们是需要通过页面组件发起action来调用reducer方法，从而改变状态，达到改变页面UI的这样一个过程。所以我们会先写一个Reducer函数，然后通过useReducer()返回给我们的state和dispatch来驱动这个数据流

```js
import React,{useReducer} from 'react'

const AddCount = () => {
const reducer = (state, action) =>  {
 if(action.type === ''add){
  return {
  ...state,
  count: state.count +1,
  }
 }else {
   return state
  }
 }
const addcount = () => { 
  dispatch({
    type: 'add'
  })
 }
const [state, dispatch] = useReducer(reducer, {count: 0})
return (
<>
<p>{state.count}</p>
<button onClick={addcount}>count++</button>
</>
)
}
export default AddCount
```

通过代码我们看到了，我们使用`useReducer()`代替了Redux的功能，但`useReducer`无法为我们提供中间件等功能，加入你有这些需求，还是需要用到redux。

##### useEffect():副作用钩子

useEffect()也是为函数组件提供了处理副作用的钩子。依然我们会把请求放在componentDidMount里面，在函数组件中我们可以使用useEffect()

```js
useEffect(() => {},[array])
```

`useEffect()`接受两个参数，第一个参数是你要进行的异步操作，第二个参数是一个数组，用来给出Effect的依赖项。只要这个数组发生变化，`useEffect()`就会执行。当第二项省略不填时，`useEffect()`会在每次组件渲染时执行。这一点类似于类组件的`componentDidMount`。



下面我们通过代码模拟一个异步加载数据。

```js
import React, { useState, useEffect } from 'react'
const AsyncPage = () => {
const [loading, setLoading] = useState(true)
  useEffect(() => {
    setTimeout(()=> {
      setLoading(false)
    },5000)
  })
return (
loading ? <p>Loading...</p>: <p>异步请求完成</p>
)
}

export default AsyncPage 
```

上面的代码实现了一个异步加载，下面我们再做一个`useEffect()`依赖第二项数组变化的例子。

```js
import React, { useState, useEffect } from 'react'

const AsyncPage = ({name}) => {
const [loading, setLoading] = useState(true)
const [person, setPerson] = useState({})

  useEffect(() => {
    setLoading(true)
    setTimeout(()=> {
      setLoading(false)
      setPerson({name})
    },2000)
  },[name])
  return (
    <>
      {loading?<p>Loading...</p>:<p>{person.name}</p>}
    </>
  )
}

const PersonPage = () =>{
  const [state, setState] = useState('')
  const changeName = (name) => {
    setState(name)
  }
  return (
    <>
      <AsyncPage name={state}/>
      <button onClick={() => {changeName('名字1')}}>名字1</button>
      <button onClick={() => {changeName('名字2')}}>名字2</button>
    </>
  )
}

export default PersonPage 
```

上面代码中，通过改变传给`AsyncPage`的props,从而调用`useEffect()`。

可以把useEffect Hook 看做如下三个函数的组合（替代了原来的生命周期）
	componentDidMount()
	componentDidupdate（）
	componentwi11unmount（）

##### useCallback和userMemo

useMemo和useCallback都会在组件第一次渲染的时候执行，之后会在其依赖的变量发生改变时再次执行；并且这两个hooks都返回缓存的值，useMemo返回缓存的变量（返回值），useCallback返回缓存的函数。

useMemo是在render期间执行的。所以不能进行一些额外的副操作，比如网络请求等。传递一个创建函数和依赖项，创建函数会需要返回一个值，只有在依赖项发生改变的时候，才会重新调用此函数，返回一个新的值。



##### ref hook

（1）Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据
（2）语法：const refcontainer=useRef（）
（3）作用：保存标签对象，功能与React.createRef（）一样

##### 创建自己的Hooks

```js
import React, { useState, useEffect } from 'react'

const usePerson = (name) => {
const [loading, setLoading] = useState(true)
const [person, setPerson] = useState({})

  useEffect(() => {
    setLoading(true)
    setTimeout(()=> {
      setLoading(false)
      setPerson({name})
    },2000)
  },[name])
  return [loading,person]
}

const AsyncPage = ({name}) => {
  const [loading, person] = usePerson(name)
    return (
      <>
        {loading?<p>Loading...</p>:<p>{person.name}</p>}
      </>
    )
  }

const PersonPage = () =>{
  const [state, setState]=useState('')
  const changeName = (name) => {
    setState(name)
  }
  return (
    <>
      <AsyncPage name={state}/>
      <button onClick={() => {changeName('名字1')}}>名字1</button>
      <button onClick={() => {changeName('名字2')}}>名字2</button>
    </>
  )
}

export default PersonPage 
```

上面代码中，我们将之前的例子封装成了自己的Hooks,便于共享。其中，我们定义`usePerson()`为我们的自定义Hooks,它接受一个字符串，返回一个数组，数组中包括两个数据的状态，之后我们在使用`usePerson()`时，会根据我们传入的参数不同而返回不同的状态，然后很简便的应用于我们的页面中。

#### react-hooks全局数据管理

##### 步骤

1. 创建全局的`rootReducer`。多个`reducer`合并到一起

2. 创建全局的`rootContext`。

3. 根组件使用`rootContext .Provider`包裹，value传递`rootReducer`的`[state, dispatch]`

4. 子组件使用`const {state,dispatch} = useContext(RootContext)`获取全局的数据

##### 代码

1. 创建全局的`rootReducer`。多个`reducer`合并到一起

- 创建两个`reducer`

```js
export const userInitState = {
  userName:'张三',
  userAge:20,
  userSex:0,
  userPhone:'15111111111'
}
export const useReducer=(state, action)=>{
  switch(action.type){
    case 'changeName':
      return {
        ...state,
        userName:action.name
      }
    case 'changeAge':
      return {
        ...state,
        userAge:action.age
      }
    default:
      return {...state}
  }
}
```

```js
export const appInitState = {
  version:'0.0.1',
  appName:'test'
}
export const appReducer=(state, action)=>{
  switch(action.type){
    case 'changeName':
      return {
        ...state,
        appName:action.name
      }
    case 'changeVersion':
      return {
        ...state,
        version:action.version
      }
    default:
      return {...state}
  }
}
```

- 创建`rootReducer`,进行合并

```js
import {useReducer, userInitState} from './userReduce'
import {appReducer, appInitState} from './appReduce'

export const rootInitState = {
  userReduce:userInitState,
  appReduce:appInitState
}

export const rootReduce = (state, action)=>{
  return {
    userReduce:useReducer(state.userReduce,action), // 注意此处传入的数据   state.userReduce 不是 state
    appReduce:appReducer(state.appReduce,action)
  }
}
```

2. 创建全局的`rootContext`

```js
import { createContext } from 'react'
export const RootContext = createContext(null)
```

3. 根组件使用`rootContext .Provider`包裹，value传递`rootReducer`的`[state, dispatch]`

```js
import { rootReduce, rootInitState } from './src/reduce/rootReduce'
import { RootContext } from './src/reduce/rootContext'

function StackNavigator() {
  const [state, dispatch] = React.useReducer(rootReduce, rootInitState)
  return <RootContext.Provider value={{state,dispatch}}>
   ...
  </RootContext.Provider>
}
```

4. 子组件使用`const {state,dispatch} = useContext(RootContext)`获取全局的数据

```js
const {state,dispatch} = useContext(RootContext) // 获取全局的数据
return (
    <View style={[styles.mainView,{backgroundColor:themesContent.color}]}>
      <Text>{state.userReduce.userName} {state.userReduce.userAge}岁</Text>
      <TouchableOpacity onPress={()=>{
        dispatch({type:'changeAge',age:21})
      }}>
        <Text>点击修改用户的年龄</Text>
      </TouchableOpacity>
      <TouchableOpacity onPress={()=>{
        dispatch({type:'changeName',name:'李四'})
      }}>
        <Text>点击修改用户的名字</Text>
      </TouchableOpacity>
...
```



## dva

#### 简介

dva 是一个基于 [redux](https://github.com/reduxjs/redux) 和 [redux-saga](https://github.com/redux-saga/redux-saga) 的数据流方案，然后为了简化开发体验，dva 还额外内置了 [react-router](https://github.com/ReactTraining/react-router) 和 [fetch](https://github.com/github/fetch)，所以也可以理解为一个轻量级的应用框架。

dva = React-Router + Redux + Redux-saga ;

#### Dva 核心概念

**基于 Redux 理念的数据流向**。 用户的交互或浏览器行为通过 dispatch 发起一个 action，如果是同步行为会直接通过 Reducers 改变 State，如果是异步行为（可以称为副作用）会先触发 Effects 然后流向 Reducers 最终改变 State。

#### dva获取与修改公共状态里的数据

获取数据

```js
import React from "react";
import { connect } from "dva";

function IndexPage(props) {
  return (
    <div>
      <h2>
        {props.name}
        {props.msg}
      </h2>
    </div>
  );
}

const mapStateToProps = state => {
  console.log(state);
  return {
    msg: "今天周末学习dva",         // 自定义的数据
    name: state.indexTest.name  // 从models 获取到的数据
  };
};
// connect 类似redux 的用法
export default connect(mapStateToProps)(IndexPage);
```

修改数据

```js
export default {
  namespace: "indexTest",
  state: {
    name: "测试"
  },
  reducers: {
    //  第一个参数代表上面的state ,第二个是传进来的参数
    setName(state, payload) {
      console.log(payload);
      const _state = JSON.parse(JSON.stringify(state)); // 必须得返回一个新的state 否则视图将不进行更新
      _state.name = payload.data.name;
      return _state;
    }
    ,
    textPath(state, payload) {
      console.log("payload");
      return state;
    }
  },
  // 异步函数，发送网络请求数据等
  effects: {          // put  发送一个action  call 发送接口可以接收两个参数（第一个请求api,第二个请求带的参数）
    *setNameAsync({ data }, { put, call }) {
      console.log(data);
      yield put({
        type: "setName", // 调用上面的方法
        data
      });
    }
  },
   // 订阅 可以进行页面加载前进行业务处理，比如拦截路由，拦截有些错误的信息传到了当前页面.....
  subscriptions: {
    text({ dispatch, history }) {
      history.listen(({ pathname }) => {
        if (pathname === "/text") {
          dispatch({
            type: "textPath"
          });
        }
      });
    }
  }
};
```

```js
import React from "react";
import { connect } from "dva";

function IndexPage(props) {
  function setName() {
    console.log(11);
    props.dispatch({
      // 命名空间加方法
      type: "indexTest/setName",
      data: {
        name: "修改方法测试"
      }
    });
  }

  function setNameAsync() {
    props.dispatch({
      type: "indexTest/setNameAsync",
      data: {
        name: "异步修改测试"
      }
    });
  }
  return (
    <div>
      <h2>
        {props.name}
        {props.msg}
      </h2>
      <button onClick={() => setName()}>修改姓名</button>
      <button onClick={() => setNameAsync()}>修改姓名异步</button>
    </div>
  );
}

const mapStateToProps = state => {
  return {
    msg: "今天周末学习dva",
    name: state.indexTest.name
  };
};

export default connect(mapStateToProps)(IndexPage);
```





## umi

`Umi`就是一个支持约定式路由和配置式路由的大杂烩，他整合了所有React生态的东西，`Antd`、`Dva`等等，把他们都当成了`Umi`的插件，我们想使用的时候，直接通过配置就能使用（里面内置集成了Dva）

目的是通过框架的方式简化 React 开发

#### umi和ant design pro

按照umi官网搭建出来的内容，相对我们要使用的项目还有很远的路要走，这个过程是持久且没有意义的，

`ant-design-pro`则为我们提供了一个模板，我们在这个模板上进行开发。

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

