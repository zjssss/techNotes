##### angular介绍

```js
谷歌开发的一款开源的web前端框架，诞生于2009年
angular是基于typescript
```

##### angular环境搭建

```js
安装nodejs
安装cnmp（非必要）
//install cli
npm install -g @angular/cli

ng new angularApp --skip-install
// install project
ng new project name
// run project
ng server --open
```

##### 创建组件

```js
ng g component componentName
```

##### typescript的class类声明属性的几种方式

```js
public  共有（默认） 可以在类里面使用，也可以在类外面使用
protected 保护类型  只有在当前类和他的子类可以访问
private 私有   只有在当前类可以访问
```

##### angular模板绑定动态数据和动态绑定属性

```html
 <div>
     {{title}}
</div>

<div [bingTitle]="title">
    
</div>
```

##### angular绑定html标签

```js
public content = "<h2>我是一个html标签</h2>"

<div [innerHTML='conten']></div>
```

##### angulr定义数组循环

```js
/∥定义数组
public arr=['1111'，'2222'，'33333'];
//推荐
public 1ist:any[]=['我是第一个新闻'，'222222222222'，'我是第三个新闻'];
public items:Array<string>=[“我是第一个新闻”，‘我是第二个新间‘];

<o1>
<1i *ngFor="let item of items">
{{item}}
</1i>
</ol>
```

##### 双向数据绑定---只是针对表单

```js
在appmodule里面引入 formmodule

<input type="text"[(ngModel)]="keywords'/>
```

##### angular动态样式

```html
<h1 class="abc" class="{{classStr}}">class1</h1>
<h1 class="abc" [class]="classstr">class2</h1>
<h1 [attr.class]="classStr">class3</h1>
```

##### 自定义管道

```
创建自定义管道文件
ng g pipe filter/lcUppercase
```



##### angular数据持久化服务（组件传值）

```js
1、创建服务在指定目录里面
ng g service services/storag
2、app.module.ts里面引入创建的服务并且声明
import{Storageservice）from './services/storage.service'
prowiders:[storageservice]
3、在用到的组件里面
//引入服务
import{storageservice）from '../../services/storage.service'；
/初始化
constructor（public storage:storageservice）{
1et s=this.storage.get（）；
 console.1og（s）；
      }
```

storage.servers.ts

```js
 import{Injectable}from‘gangular/core'；
 @eInjectable({
providedIn:"root"
 })
 export class StorageService{
  constructor(){}
     set(key:any,value:any){
	  1ocalstorage. setItem(key,JSON.stringify(value))
     };
	
     get(key:any,value:any){
         return JSON.parse(localStrage.getItem(key))
     }
     
	remove(key:any){
	1ocalstorage. removeItem(key);
   }
```

##### angular中的dom操作

```js
ViewChild获取dom节点
1、模板中给dom起一个名字
<div #myBox> 我是一个dom节点 </div>
2、在业务逻辑里面引入viewChild 
import{Component，OnInit，ViewChild}from‘@angular/core'；
3、写在类里面@viewChild（'myBox'）myBox:any；
4、ngAfterViewInit生命周期函数里面获取dom 
this.myBox.nativeElement I
```

##### angular组件传值

父组件模板传递

```html
<app-header [title]='title' [msg]='msg' [home]='this'></app-header>
```

子组件接收

```js
子组件引入Input
import { Component,OnInit,Input} from'gangular/core';


@input() title:any;   接收数据
@input() msg:any;	  接收方法
@input() home:any;	  接收整个父组件
```



父组件主动获取子组件的数据和方法

```json
<app-footer #footer></app-footer>

@viewChild('footer') footer:any;

this.footer.msg
this.footer.run()
```

子组件调用父组件的数据和方法

子组件

```json
引入outputd
import{Component,OnInit,Outputd,EventEmitter }from 'gangular/core';

@0utput() private outer = new EventEmitter();

this.outer.emit('我是子组件的数据')
```

父组件的html文件

```html
<app-footer (outer)='run(event)'></app-footer>
```

父组件ts文件

```js
run(event){
	console.log(event)  //子组件给父组件广播的数据   我是子组件的数据
}
```

生命周期

```
ngOnChanges()
ngOnInit()
ngDoCheck()
ngAfterContentInit()
ngAfterContentChecked()
ngAfterViewInit()
ngAfterViewChecked()
ngOnDestroy()
```

##### ngular内置过滤器一共有几种，分别是那些？

　　date：日期格式化

　　currency：货币 

　　uppercase：大写

　　lowercase：小写

　　limitTo（限制数组或字符串长度）

　　orderBy（排序）

　　number（格式化数字，加上千位分隔符，并接收参数限定小数点位数）

　　filter（处理一个数组，过滤出含有某个子串的元素）

　　json（格式化 json 对象）

##### angular 核心？

　　AngularJS是为了克服HTML在构建应用上的不足而设计的。 AngularJS有着诸多特性，最为核心的是：

- MVC
- 模块化
- 自动化双向数据绑定
- 语义化标签、依赖注入等等

##### angular的数据绑定采用什么机制？详述原理

　脏检查机制

双向数据绑定是 AngularJS 的核心机制之一。当 view 中有任何数据变化时，会更新到 model ，当 model 中数据有变化时，view 也会同步更新，显然，这需要一个监控。

原理就是，Angular 在 scope 模型上设置了一个监听队列，用来监听数据变化并更新 view 。每次绑定一个东西到 view 上时 AngularJS 就会往 $watch 队列里插入一条 $watch ，用来检测它监视的 model 里是否有变化的东西。当浏览器接收到可以被 angular context 处理的事件时， $digest 循环就会触发，遍历所有的 $watch ，最后更新 dom。

##### **AngularJS 的数据双向绑定是怎么实现的**

1、每个双向绑定的元素都有一个 watcher

2、在某些事件发生的时候，调用 digest 脏数据检测。

这些事件有：表单元素内容变化、Ajax 请求响应、点击按钮执行的函数等。

3、脏数据检测会检测 rootscope 下所有被 watcher 的元素。

$digest 函数就是脏数据监测

##### ng-show/ng-hide 与 ng-if 的区别

答案：我们都知道 ng-show/ng-hide 实际上是通过 display 来进行隐藏和显示的。而 ng-if 实际上控制 dom 节点的增删除来实现的。因此如果我们是根据不同的条件来进行 dom 节点的加载的话，那么 ng-if 的性能好过 ng-show.

##### 什么是rootScrope以及和scope 的区别

通俗地讲rootScrope页面是所有scope的父亲

step1:Angular 解析 ng-app 然后在内存中创建$rootScope。

step2:angular 回继续解析，找到{{}}表达式，并解析成变量。

step3:接着会解析带有 ng-controller 的 div 然后指向到某个 controller 函数。 这个时候在这个 controller 函数变成一个$scope 对象实例。

##### 列出至少三种实现不同模块之间通信方式

- Service
- events,指定绑定的事件
- 使用 $rootScope
- controller 之间直接使用$parent, $$childHead 等
- directive 指定属性进行数据绑定

