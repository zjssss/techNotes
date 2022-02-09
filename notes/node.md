## node主流框架

express  koa（egg.js是封装了一套koa，可以理解成大礼包版koa，集成度高，可以轻松创建一个项目而不用做很多繁琐的初期工作，解放生产力，更可贵的是有一套现成的规范提供给我们，不需要我们自己再去探索一套规范，比如router放哪里，controller放哪里，需不需要service，哪些放在service等等）

## express和koa的区别

### 使用区别

从初始化就看出koa语法都是用的新标准。在挂载路由中间件上也有一定的差异性，这是因为二者内部实现机制的不同。其他都是大同小异的了

|                  | koa(Router = require('koa-router')) | **express(假设不使用app.get之类的方法)** |
| ---------------- | ----------------------------------- | ---------------------------------------- |
| 初始化           | const app = new koa()               | const app = express()                    |
| 实例化路由       | const router = Router()             | const router = express.Router()          |
| app级别的中间件  | app.use                             | app.use                                  |
| 路由级别的中间件 | router.get                          | router.get                               |
| 路由中间件挂载   | app.use(router.routes())            | app.use('/', router)                     |
| 监听端口         | pp.listen(3000)                     | pp.listen(3000)                          |

### 语法区别

experss 异步使用 回调
koa1 异步使用 generator + yeild
koa2 异步使用 await/async

### 中间件区别

koa采用洋葱模型，进行顺序执行，出去反向执行，支持context传递数据
express本身无洋葱模型，需要引入插件，不支持context
express的中间件中执行异步函数，执行顺序不会按照洋葱模型，异步的执行结果有可能被放到最后，response之前。
这是由于，其中间件执行机制，递归回调中没有等待中间件中的异步函数执行完毕，就是没有await中间件异步函数

### 集成度区别

express 内置了很多中间件，集成度高，使用省心，
koa 轻量简洁，容易定制

#### 

