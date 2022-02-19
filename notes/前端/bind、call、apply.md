#### 共同点

改变函数执行时的this指向，目前所有关于它们的运用，都是基于这一点来进行的

三者都指向其接收的第一个参数，为`null`或者`undefined`时则为`window`对象

call、apply和bind是挂在Function对象上（函数原型链）的三个方法,只有函数才有这些方法

#### 核心理念

call/apply/bind的核心理念：借用方法

借助已实现的方法，改变方法中数据的this指向，减少重复代码，节省内存

#### 区别

（1）`apply`和`call`都是直接调用函数，而`bind`则是先将函数暂存起来，需要再单独调用一次

（2）接收参数的形式不一致

apply第二个参数是一个数组

bind和call 则是接受一系列的单独变量

```js
fn.call(obj, 1, 2); // 改变fn中的this，并且把fn立即执行
fn.bind(obj, 1, 2); // 改变fn中的this，fn并不执行
fn.apply(obj, [1, 2]);// 改变fn中的this，并且把fn立即执行
```

#### **返回值**

- call/apply 返回`fun`的执行结果
- bind返回fun的拷贝，并指定了fun的this指向，保存了fun的参数。

#### call、apply，该用哪个

call,apply的效果完全一样，它们的区别也在于

- **参数数量/顺序确定就用call，参数数量/顺序不确定的话就用apply**。
- 考虑可读性：参数数量不多就用call，参数数量比较多的话，把参数整合成数组，使用apply。
- 参数集合已经是一个数组的情况，用apply，比如上文的获取数组最大值/最小值。



#### 应用场景

##### 数组最大和最小值（apply）

```js
var arr = [0,8,3,46]
var max = Math.max.apply(null,arr);//46
var min = Math.min.apply(null,arr);//0
// 等价于
var max = window.Math.max(...arr);
var min = window.Math.min(...arr);
```

```js
// 有只猫叫小黑，小黑会吃鱼
const cat = {
    name: '小黑',
    eatFish(...args) {
        console.log('this指向=>', this);
        console.log('...args', args);
        console.log(this.name + '吃鱼');
    },
}
// 有只狗叫大毛，大毛会吃骨头
const dog = {
    name: '大毛',
    eatBone(...args) {
        console.log('this指向=>', this);
        console.log('...args', args);
        console.log(this.name + '吃骨头');
    },
}

console.log('=================== call =========================');
// 有一天大毛想吃鱼了，可是它不知道怎么吃。怎么办？小黑说我吃的时候喂你吃
cat.eatFish.call(dog, '汪汪汪', 'call')
// 大毛为了表示感谢，决定下次吃骨头的时候也喂小黑吃
dog.eatBone.call(cat, '喵喵喵', 'call')

console.log('=================== apply =========================');
cat.eatFish.apply(dog, ['汪汪汪', 'apply'])
dog.eatBone.apply(cat, ['喵喵喵', 'apply'])

console.log('=================== bind =========================');
// 有一天他们觉得每次吃的时候再喂太麻烦了。干脆直接教对方怎么吃
const test1 = cat.eatFish.bind(dog, '汪汪汪', 'bind')
const test2 = dog.eatBone.bind(cat, '喵喵喵', 'bind')
test1()
test2()
```

##### 类数组借用数组的方法（call）

类数组因为不是真正的数组所有没有数组类型上自带的种种方法，所以我们需要去借用数组的方法。

比如借用数组的push方法：

```js
var arrayLike = {
  0: 'OB',
  1: 'Koro1',
  length: 2
}
Array.prototype.push.call(arrayLike, '添加元素1', '添加元素2');
console.log(arrayLike) // {"0":"OB","1":"Koro1","2":"添加元素1","3":"添加元素2","length":4}
```



##### 判断数据类型（call）

```js
function isType(data, type) {
    const typeObj = {
        '[object String]': 'string',
        '[object Number]': 'number',
        '[object Boolean]': 'boolean',
        '[object Null]': 'null',
        '[object Undefined]': 'undefined',
        '[object Object]': 'object',
        '[object Array]': 'array',
        '[object Function]': 'function',
        '[object Date]': 'date', // Object.prototype.toString.call(new Date())
        '[object RegExp]': 'regExp',
        '[object Map]': 'map',
        '[object Set]': 'set',
        '[object HTMLDivElement]': 'dom', // document.querySelector('#app')
        '[object WeakMap]': 'weakMap',
        '[object Window]': 'window',  // Object.prototype.toString.call(window)
        '[object Error]': 'error', // new Error('1')
        '[object Arguments]': 'arguments',
    }
    let name = Object.prototype.toString.call(data) // 借用Object.prototype.toString()获取数据类型
    let typeName = typeObj[name] || '未知类型' // 匹配数据类型
    return typeName === type // 判断该数据类型是否为传入的类型
}
console.log(
    isType({}, 'object'), // true
    isType([], 'array'), // true
    isType(new Date(), 'object'), // false
    isType(new Date(), 'date'), // true
)

作者：OBKoro1
链接：https://juejin.cn/post/6844903906279964686
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

##### 保存函数参数（bind）

回调函数this丢失

```js
this.pageClass = new Page(this.handleMessage.bind(this)) // 绑定回调函数的this指向
```

