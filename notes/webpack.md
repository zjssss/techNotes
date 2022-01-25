### 官网

https://webpack.docschina.org/

### 定义

Webpack 是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。

Webpack 可以将多种静态资源 js、css、less 转换成一个静态文件，减少了页面的请求

```js
npm install webpack webpack-dev-server --save-dev
```

### 优点

#### 代码层面

提供更小的项目体积（Tree-shaking、压缩、合并），加载更快

编译浏览器不能识别的高级语言和语法（TS、ES6、模块化、scss）

可以提高浏览器的兼容性和错误检查（polyfill,postcss,eslint）

#### 研发流程层面

可以实现统一、高效的开发环境

可以实现统一的构建流程和产出标准

可以集成公司构建规范（提测、上线）

### 入口出口

webpack可以有多个入口
当我们需要多个入口文件的时候，可以把entry写成一个对象

```js
var baseConfig = {
        entry: './src/index.js'
    }
    var baseConfig = {
        entry: {
            main: './src/index.js'
        }
    }
```

output: 即使入口文件有多个，但是只有一个输出配置，就是说只有一个出口

```js
var path = require('path')
    var baseConfig = {
        entry: {
            main: './src/index.js'
        },
        output: {
            filename: 'main.js',
            path: path.resolve('./build')
        }
    }
    module.exports = baseConfig
```

如果你定义的入口文件有多个，那么我们需要使用占位符来确保输出文件的唯一性

```js
output: {
        filename: '[name].js',
        path: path.resolve('./build')
    }
```



### module，chunk和bundle的关系

1. 对于一份同逻辑的代码，当我们手写下一个一个的文件，它们无论是 ESM 还是 commonJS 或是 AMD，他们都是 **module** ；

2. 当我们写的 module 源文件传到 webpack 进行打包时，webpack 会根据文件引用关系生成 **chunk** 文件，webpack 会对这个 chunk 文件进行一些操作；

3. webpack 处理好 chunk 文件后，最后会输出 **bundle** 文件，这个 bundle 文件包含了经过加载和编译的最终源文件，所以它可以直接在浏览器中运行。

   `module`，`chunk` 和 `bundle` 其实就是同一份逻辑代码在不同转换场景下的取了三个名字：

   我们直接写出来的是 module，webpack 处理时是 chunk，最后生成浏览器可以直接运行的 bundle。

### webpack和loader和plugin的关系

- `Babel`和`webpack`是独立的工具
- 二者可以一起工作
- 二者都可以通过插件拓展功能

### Loader和Plugin的不同

#### 不同的作用

Loader直译为加载器
Webpack将一切文件视为模块，但是webpack原生是只能解析js文件，如果想将其他文件也打包的话，就会用到loader。 所以Loader的作用是让webpack拥有了加载和解析非JavaScript文件的能力

Plugin直译为"插件"
目的在于解决loader无法实现的其他事，从打包优化和压缩，到重新定义环境变量，功能强大到可以用来处理各种各样的任务
Plugin可以扩展webpack的功能，让webpack具有更多的灵活性。
 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

#### 不同的用法

- **Loader**需要在webpack.config.js里边单独用module.rules中配置，也就是说他作为模块的解析规则而存在。 类型为数组，每一项都是一个Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options）
- **Plugin**在plugins中单独配置。 类型为数组，每一项是一个plugin的实例，参数都通过构造函数传入。

### 有哪些常见的Loader？他们是解决什么问题的？

- file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
- url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
- source-map-loader：加载额外的 Source Map 文件，以方便断点调试
- image-loader：加载并且压缩图片文件
- babel-loader：把 ES6 转换成 ES5，让浏览器能够支持js文件。babel有些复杂，所以大多数都会新建一个.babelrc进行配置
- css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
- style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
- eslint-loader：通过 ESLint 检查 JavaScript 代码

```js
var baseConfig = {
                entry: {
                    main: './src/index.js'
                },
                output: {
                    filename: '[name].js',
                    path: path.resolve('./build')
                },
                devServer: {
                    contentBase: './src',
                    historyApiFallBack: true,
                    inline: true
                },
                module: {
                    rules: [
                        {
                            test: /\.less$/,
                            use: [
                                {loader: 'style-loader'},
                                {loader: 'css-loader'},
                                {loader: 'less-loader'}
                            ],
                            exclude: /node_modules/
                        }
                    ]
                }
            }
```



### 有哪些常见的Plugin？他们是解决什么问题的？

define-plugin：定义环境变量

```js
var ENV = process.env.NODE_ENV
    var baseConfig = {
        // ... 
        plugins: [
            new webpack.DefinePlugin({
                'process.env.NODE_ENV': JSON.stringify(ENV)
            })
        ]
    }
```

commons-chunk-plugin：提取公共代码

uglifyjs-webpack-plugin：通过UglifyES压缩ES6代码

```js
var baseConfig = {
 new webpack.optimize.OccurenceOrderPlugin()
 new webpack.optimize.UglifyJsPlugin()
}
```

ExtractTextWebpackPlugin: 它会将入口中引用css文件，都打包都独立的css文件中，而不是内嵌在js打包文件中

```js
var ExtractTextPlugin = require('extract-text-webpack-plugin')
        var lessRules = {
            use: [
                {loader: 'css-loader'},
                {loader: 'less-loader'}
            ]
        }
        
        var baseConfig = {
            // ... 
            module: {
                rules: [
                    // ...
                    {test: /\.less$/, use: ExtractTextPlugin.extract(lessRules)}
                ]
            },
            plugins: [
                new ExtractTextPlugin('main.css')
            ]
        }
```



HtmlWebpackPlugin：依据一个简单的index.html模版，生成一个自动引用你打包后的js文件的新index.html

```js
var HTMLWebpackPlugin = require('html-webpack-plugin')
            var baseConfig = {
                // ...
                plugins: [
                    new HTMLWebpackPlugin()
                ]
            }
```



HotModuleReplacementPlugin：允许你在修改组件代码时，自动刷新实时预览修改后的结果注意永远不要在生产环境中使用HMR。这儿说一下一般情况分为开发环境，测试环境，生产环境（热更新）

### webpack热更新

#### 概念

webpack的热更新又称热替换（Hot Module Replacement），缩写为HMR。 这个机制可以做到不用刷新浏览器而将新变更的模块替换掉旧的模块。

模块热替换是 webpack 提供的最有用的功能之一。它允许在运行时替换，添加，删除各种模块，而无需进行完全刷新重新加载整个页面，其思路主要有以下几个方面

（1）保留在完全重新加载页面时丢失的应用程序的状态

（2）只更新改变的内容，以节省开发时间

（3）调整样式更加快速，几乎等同于就在浏览器调试器中更改样式

模块热更新(热替换)，其目的是为了加快用户的开发速度，提高编程体验。它并不适用于生产环境，这意味着它应当只在开发环境使用

#### 用法示例

HMR的启用十分简单，这归功于webpack内置插件使用上的便利。我们要做的仅仅是更新[webpack-dev-server](https://github.com/webpack/webpack-dev-server)的配置，和使用webpack内置的HMR插件

1. 引入了webpack库
2. 使用了`new webpack.HotModuleReplacementPlugin()`
3. 设置`devServer`选项中的`hot`字段为`true`

```js
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const CleanWebpackPlugin = require('clean-webpack-plugin');
  const webpack = require('webpack');

  module.exports = {
    entry: {
      app: './src/index.js'
    },
    devtool: 'inline-source-map',
    devServer: {
      contentBase: './dist',
      hot: true
    },
    plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        title: 'Hot Module Replacement'
      }),
      new webpack.HotModuleReplacementPlugin()
    ],
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```

### 如何提高webpack的构建速度？

1. 多入口情况下，使用CommonsChunkPlugin来提取公共代码
2. 通过externals配置来提取常用库
3. 利用DllPlugin和DllReferencePlugin预编译资源模块 通过DllPlugin来对那些我们引用但是绝对不会修改的npm包来进行预编译，再通过DllReferencePlugin将预编译的模块加载进来。
4. 使用Happypack 实现多线程加速编译
5. 使用webpack-uglify-parallel来提升uglifyPlugin的压缩速度。 原理上webpack-uglify-parallel采用了多核并行压缩来提升压缩速度
6. 使用Tree-shaking和Scope Hoisting来剔除多余代码

### 如何利用webpack来优化前端性能？（提高性能和体验）

用webpack优化前端性能是指优化webpack的输出结果，让打包的最终结果在浏览器运行快速高效。

- 压缩代码。删除多余的代码、注释、简化代码的写法等等方式。可以利用webpack的UglifyJsPlugin和ParallelUglifyPlugin来压缩JS文件， 利用cssnano（css-loader?minimize）来压缩css
- 利用[CDN](https://cloud.tencent.com/product/cdn?from=10680)加速。在构建过程中，将引用的静态资源路径修改为CDN上对应的路径。可以利用webpack对于output参数和各loader的publicPath参数来修改资源路径
- 删除死代码（Tree Shaking）。将代码中永远不会走到的片段删除掉。可以通过在启动webpack时追加参数--optimize-minimize来实现
- 提取公共代码（commons-chunk-plugin）。

### webpack的构建流程是什么?从读取配置到输出文件这个过程尽量说全

Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：

1. 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
2. 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
3. 确定入口：根据配置中的 entry 找出所有的入口文件；
4. 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
5. 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
6. 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；
7. 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

在以上过程中，Webpack 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。

### webpack与grunt、gulp的不同？

三者都是前端构建工具，grunt和gulp在早期比较流行，现在webpack相对来说比较主流，不过一些轻量化的任务还是会用gulp来处理，比如单独打包CSS文件等。

grunt和gulp是基于任务和流（Task、Stream）的。类似jQuery，找到一个（或一类）文件，对其做一系列链式操作，更新流上的数据， 整条链式操作构成了一个任务，多个任务就构成了整个web的构建流程。

webpack是基于入口的。webpack会自动地递归解析入口所需要加载的所有资源文件，然后用不同的Loader来处理不同的文件，用Plugin来扩展webpack功能。

所以总结一下：

- 从构建思路来说

gulp和grunt需要开发者将整个前端构建过程拆分成多个`Task`，并合理控制所有`Task`的调用关系 webpack需要开发者找到入口，并需要清楚对于不同的资源应该使用什么Loader做何种解析和加工

- 对于知识背景来说

gulp更像后端开发者的思路，需要对于整个流程了如指掌 webpack更倾向于前端开发者的思路

### webpack如何优化vue项目

#### 优化loader配置

由于Loader对文件的转换操作很耗时，所以需要让尽可能少的文件被Loader处理。
我们可以通过以下3方面优化Loader配置：（1）优化正则匹配（2）通过cacheDirectory选项开启缓存（3）通过include、exclude来减少被处理的文件

```js
//原配置
{
  test: /\.js$/,
  loader: 'babel-loader',
  include: [resolve('src'), resolve('test')]
},

//优化后的配置
{
  // 1、如果项目源码中只有js文件，就不要写成/\.jsx?$/，以提升正则表达式的性能
  test: /\.js$/,
  // 2、babel-loader支持缓存转换出的结果，通过cacheDirectory选项开启
  loader: 'babel-loader?cacheDirectory',
  // 3、只对项目根目录下的src 目录中的文件采用 babel-loader
  include: [resolve('src')]
},
```

#### 优化UglifyJS插件

webpack默认提供了UglifyJS插件来压缩JS代码，但是它使用的是单线程压缩代码，也就是说多个js文件需要被压缩，它需要一个个文件进行压缩。所以说在正式环境打包压缩代码速度非常慢

因此我们需要可以并行处理多个子任务，多个子任务完成后，再将结果发到主进程中，有了这个思想后，因此 ParallelUglifyPlugin 插件就产生了，当webpack有多个JS文件需要输出和压缩时候，原来会使用UglifyJS去一个个压缩并且输出，但是ParallelUglifyPlugin插件则会开启多个子进程，把对多个文件压缩的工作分别给多个子进程去完成，但是每个子进程还是通过UglifyJS去压缩代码。无非就是变成了并行处理该压缩了，并行处理多个子任务，效率会更加的提高。

打包对比基本上大小能相差30%！！！！

```js
 安装 webpack-parallel-uglify-plugin 插件
然后在webpack.config.js 配置代码如下：
// 引入 ParallelUglifyPlugin 插件
const ParallelUglifyPlugin = require('webpack-parallel-uglify-plugin');

new ParallelUglifyPlugin({})

```

#### 减少冗余代码

babel-plugin-transform-runtime 是Babel官方提供的一个插件，作用是减少冗余的代码 。 Babel在将ES6代码转换成ES5代码时，通常需要一些由ES5编写的辅助函数来完成新语法的实现，例如在转换 class extent 语法时会在转换后的 ES5 代码里注入 extent 辅助函数用于实现继承。babel-plugin-transform-runtime会将相关辅助函数进行替换成导入语句，从而减小babel编译出来的代码的文件大小

#### DllPlugin分包

通过DllPlugin插件分离出第三方包

新建webpack.dll.conf.js

```js
 plugins: [
    new CleanWebpackPlugin({
      root: path.resolve(__dirname, '../static/dll'),
      dry: false // 启用删除文件
    }),
    new webpack.DllPlugin({
      name: '[name]_dll_[hash:6]',
      path: path.resolve(__dirname, '../static/dll', '[name].dll.manifest.json')
    })
  ]

```

#### 按需加载代码

**路由懒加载**

通过vue写的单页应用时，可能会有很多的路由引入。当打包构建的时候，javascript包会变得非常大，影响加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应的组件，这样就更加高效了。这样会大大提高首屏显示的速度，但是可能其他的页面的速度就会降下来。

#### 提取公共代码

如果每个页面的代码都将这些公共的部分包含进去，则会造成以下问题 ：
• 相同的资源被重复加载，浪费用户的流量和服务器的成本。
• 每个页面需要加载的资源太大，导致网页首屏加载缓慢，影响用户体验。
如果将多个页面的公共代码抽离成单独的文件，就能优化以上问题 。
Webpack内置了专门用于提取多个Chunk中的公共部分的插件CommonsChunkPlugin。

项目中CommonsChunkPlugin的配置：
所有在 package.json 里面依赖的包，都会被打包进 vendor.js 这个文件中。

```js
new webpack.optimize.CommonsChunkPlugin({
  name: 'vendor',
  minChunks: function(module, count) {
    return (
      module.resource &&
      /\.js$/.test(module.resource) &&
      module.resource.indexOf(
        path.join(__dirname, '../node_modules')
      ) === 0
    );
  }
}),
// 抽取出代码模块的映射关系
new webpack.optimize.CommonsChunkPlugin({
  name: 'manifest',
  chunks: ['vendor']
}),
```

#### productionSourceMap

productionSourceMap设置为false，不然最终打包过后会生成一些map文件

#### gzip压缩

开启gzip压缩，使打包过后体积变小

#### image-webpack-loader

使用image-webpack-loader插件来压缩图片