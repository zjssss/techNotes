#### es6（其中一些包括es6之后的版本）

##### 变量声明

###### let 和 const

ES5 只有全局作用域和函数作用域，没有块级作用域

`let`实际上为 JavaScript 新增了块级作用域

ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于`let`，在块级作用域之外不可引用。

`const`声明的变量不得改变值，这意味着，`const`一旦声明变量，就必须立即初始化，不能留到以后赋值。

###### 声明变量的六种方法

ES5 只有两种声明变量的方法：`var`命令和`function`命令。ES6 除了添加`let`和`const`命令，后面章节还会提到，另外两种声明变量的方法：`import`命令和`class`命令。所以，ES6 一共有 6 种声明变量的方法

##### 解构赋值

###### 数组的结构赋值

本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值

如果解构不成功，变量的值就等于`undefined`

```js
let [a, b, c] = [1, 2, 3];

let [foo, [[bar], baz]] = [1, [[2], 3]]

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

//默认值
let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

###### 对象解构赋值

对象的解构与数组有一个重要的不同。

数组的元素是按次序排列的，变量的取值由它的位置决定；

而对象的属性没有次序，变量必须与属性同名，才能取到正确的值

```js
let { foo, bar } = { foo: 'aaa', bar: 'bbb' };

let { baz } = { foo: 'aaa', bar: 'bbb' };
baz // undefined

//默认值
var {x: y = 3} = {x: 5};
```

##### 字符串的结构赋值

此时，字符串被转换成了一个类似数组的对象

```js
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值
let {length : len} = 'hello';
len // 5
```

##### 函数参数的解构赋值

```js
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3

```

##### 解构赋值的用途

###### 交换变量的值

```js
let x = 1;
let y = 2;

[x, y] = [y, x];
```

###### **从函数返回多个值**

从函数返回多个值的时候，可以用解构赋值取出需要的的值

###### 函数参数的定义

解构赋值可以方便地将一组参数与变量名对应起来。

```js
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

###### **提取 JSON 数据**

解构赋值对提取 JSON 对象中的数据，尤其有用

###### **函数参数的默认值**

指定参数的默认值，就避免了在函数体内部再写`var foo = config.foo || 'default foo';`这样的语句

```js
下面是代码案例，以对象解构赋值为案例，其他的还有数组解构暂时不讲：
<script>
		function move( {x=0,y=0} = {x:3,y:4} ){
			console.log(x,y)
			return x+y;
		}
		move({x:1,y:2});// 1 2
		move({}) //0 0
		move() //3 4
</script>

```

1、首先判断是否有传参过去，如果有的话就使用传入的参数(此案例参数必须是对象)，
2、如果没有传入实参的话就使用参数默认值，
3、如果有参数，但是没有具体值的话，就使用解构赋值的默认值。

###### **输入模块的指定方法**

加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。

```js
const { SourceMapConsumer, SourceNode } = require("source-map");
```

##### 字符串的扩展

###### 模板字符串

1 可以多行拼接
2 可以在拼接中插入变量
3 可以进行简单的运算
4 可以互相嵌套
5 简单，方便，整洁

模板字符串内可以放入表达式，函数调用

##### 字符串新增方法

###### **includes()**

返回布尔值，表示是否找到了参数字符串。

###### **startsWith()**

返回布尔值，表示参数字符串是否在原字符串的头部。

###### **endsWith()**

返回布尔值，表示参数字符串是否在原字符串的尾部。

###### **repeat()**

方法返回一个新字符串，表示将原字符串重复`n`次

###### padStart()，padEnd()

ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。`padStart()`用于头部补全，`padEnd()`用于尾部补全。

一共接受两个参数，第一个参数是字符串补全生效的最大长度，第二个参数是用来补全的字符串。

如果原字符串的长度，等于或大于最大长度，则字符串补全不生效，返回原字符串

```js
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```

###### trimStart()，trimEnd()

`trimStart()`消除字符串头部的空格，`trimEnd()`消除尾部的空格。

它们返回的都是新字符串，不会修改原始字符串。

trim()是消除左右的空格

###### replaceAll()

字符串的实例方法`replace()`只能替换第一个匹配

`replaceAll()`方法，可以一次性替换所有匹配。它返回一个新字符串，不会改变原字符串

正则表达式的`g`修饰符也可以一次性替换所有匹配

##### 函数的扩展

###### 函数参数的默认值 

参数变量是默认声明的，在函数体中，不能用`let`或`const`再次声明，否则会报错。

使用参数默认值时，函数不能有同名参数

参数默认值不是传值的，而是每次都重新计算默认值表达式的值。

也就是说，参数默认值是惰性求值的。

与解构赋值默认值结合使用

通常情况下，定义了默认值的参数，应该是函数的尾参数

如果非尾部的参数设置默认值，实际上这个参数是没法省略的。

###### rest 参数（剩余参数）

ES6 引入 rest 参数（形式为`...变量名`），用于获取函数的多余参数，

这样就不需要使用`arguments`对象了。

rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错

###### 箭头函数

箭头函数没有自己的`this`对象，内部的`this`就是定义时上层作用域中的`this`。

也就是说它统一了this的指向

不可以当作构造函数，也就是说，不可以对箭头函数使用`new`命令

###### name 属性

函数的`name`属性，返回该函数的函数名

##### 数组的扩展

###### 扩展运算符

扩展运算符（spread）是三个点（`...`）

该运算符主要用于函数调用。

扩展运算符后面还可以放置表达式

```js
const arr = [
  ...(x > 0 ? ['a'] : []),
  'b',
];
```



###### 扩展运算符的应用

复制数组,不会对原数组造成影响

合并数组，不会对原数组造成影响

复制数组合并数组都属于浅拷贝

与解构赋值结合

```js
// ES5
a = list[0], rest = list.slice(1)
// ES6
[a, ...rest] = list
```

###### Array.from()

`Array.from`方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象

实际应用中，常见的类似数组的对象是 DOM 操作返回的 NodeList 集合，以及函数内部的`arguments`对象。`Array.from`都可以将它们转为真正的数组。

例子：下面的例子是取出一组 DOM 节点的文本内容

```js
let spans = document.querySelectorAll('span.name');
let names2 = Array.from(spans, s => s.textContent)
```

###### Array.of()

`Array.of()`方法用于将一组值，转换为数组

###### find() 和 findIndex() 

数组实例的`find`方法，用于找出第一个符合条件的数组成员,如果没有符合条件的成员，则返回`undefined`

数组实例的`findIndex`方法,返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回`-1`

这两个的参数都是 一个回调函数

###### entries()，keys() 和 values()

ES6 提供三个新的方法用于遍历数组，它们都返回一个遍历器对象，可以用`for...of`循环进行遍历

`keys()`是对键名的遍历

`values()`是对键值的遍历

`entries()`是对键值对的遍历

###### includes()

表示某个数组是否包含给定的值，与字符串的`includes`方法类似

###### flat()，flatMap()

flat() 用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响

`flat()`默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将`flat()`方法的参数写成一个整数，表示想要拉平的层数，默认为1

如果不管有多少层嵌套，都要转成一维数组，可以用`Infinity`关键字作为参数

`flatMap()`方法对原数组的每个成员执行一个函数（相当于执行`Array.prototype.map()`），然后对返回值组成的数组执行`flat()`方法。该方法返回一个新数组，不改变原数组。

```js
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]
```



##### 对象的扩展

###### 属性简写

键值对名字一样的情况下，可以只写一个

###### 方法简写

在对象内，一个属性是方法的话，可以省略function

###### 属性名表达式

用表达式作为属性名，这时要将表达式放在方括号之内

###### 属性的遍历

ES6 一共有 5 种方法可以遍历对象的属性

`for...in`遍历对象自身的和原型链上面的可枚举属性（不含 Symbol 属性）（obj.hasOwnProperty(i)过滤原型上的属性）

`Object.keys`返回一个数组，遍历对象自身的（不含原型链上面的可枚举属性）所有可枚举属性（不含 Symbol 属性）的键名。

`Object.getOwnPropertyNames`返回一个数组，遍历对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名

`Object.getOwnPropertySymbols`返回一个数组，包含对象自身的所有 Symbol 属性的键名。

`Reflect.ownKeys`返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。

注意：开发者自定义的属性在一般情况下时可以枚举的，但是内置的对象Math和基本包装类型里的属性是不可枚举的

##### 枚举属性和不可枚举属性

在JavaScript中，对象的属性分为可枚举和不可枚举之分，它们是由属性的enumerable值决定的。
可枚举性决定了这个属性能否被for…in查找遍历到

js中基本包装类型的原型属性是不可枚举的，如Object, Array, Number等

###### 解构赋值

###### 扩展运算符

##### 对象的新增方法

###### Object.is

ES6 提出“Same-value equality”（同值相等）算法，用来解决这个问题。`Object.is`就是部署这个算法的新方法。它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

用于其他不同类型的严格比较

```js
Object.is('foo', 'foo')
// true
Object.is({}, {})
// false
```



###### Object.assign()

用于一个或多个对象的合并，属于浅拷贝

如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

常见用途：

（1）为对象添加属性

（2）为对象添加方法

（3）克隆对象

（4）合并多个对象

###### __ proto __属性

用来读取或设置当前对象的原型对象（prototype）

###### Object.keys(), Object.values()，Object.entries()

相同点：都是用于对象的遍历；都是返回一个数组；不能遍历原型上的属性

ES5 引入了`Object.keys`方法，返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的**键名(属性)**

```js
var obj = { foo: 'bar', baz: 42 };
Object.keys(obj)
// ["foo", "baz"]
```

ES2017 引入了跟`Object.keys`配套的`Object.values`**（键值）**和`Object.entries`**（键值对）**

作为遍历一个对象的补充手段，供`for...of`循环使用

`Object.values`方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的**键值(属性的值)**。

```js
const obj = { foo: 'bar', baz: 42 };
Object.values(obj)
// ["bar", 42]
```

`Object.entries()`方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。

```js
const obj = { foo: 'bar', baz: 42 };
Object.entries(obj)
// [ ["foo", "bar"], ["baz", 42] ]
```

##### symble

###### 定义

Symbol 值通过`Symbol`函数生成；
它是 JavaScript 语言的第七种原始数据类型，表示独一无二的值。
symble属于js的基本数据类型

前六种是：`undefined`、`null`、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）



ES5 的对象属性名都是字符串，这容易造成属性名的冲突。

这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。

由于每一个 Symbol 值都是不相等的，这意味着 Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。

注意：在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中

```js
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```

###### 应用场景

消除魔术字符串

魔术字符串指的是，在代码之中多次出现、与代码形成强耦合的某一个具体的字符串或者数值。风格良好的代码，应该尽量消除魔术字符串，改由含义清晰的变量代替。

```js
function getArea(shape, options) {
  let area = 0;

  switch (shape) {
    case 'Triangle': // 魔术字符串
      area = .5 * options.width * options.height;
      break;
    /* ... more code ... */
  }

  return area;
}

getArea('Triangle', { width: 100, height: 100 }); // 魔术字符串
```

上面代码中，字符串`Triangle`就是一个魔术字符串。它多次出现，与代码形成“强耦合”，不利于将来的修改和维护。

常用的消除魔术字符串的方法，就是把它写成一个变量。

```js
const shapeType = {
  triangle: 'Triangle'
};

function getArea(shape, options) {
  let area = 0;
  switch (shape) {
    case shapeType.triangle:
      area = .5 * options.width * options.height;
      break;
  }
  return area;
}

getArea(shapeType.triangle, { width: 100, height: 100 });
```

上面代码中，我们把`Triangle`写成`shapeType`对象的`triangle`属性，这样就消除了强耦合。

如果仔细分析，可以发现`shapeType.triangle`等于哪个值并不重要，只要确保不会跟其他`shapeType`属性的值冲突即可。因此，这里就很适合改用 Symbol 值。

```js
const shapeType = {
  triangle: Symbol()
};
```

属性名的遍历

Symbol 作为属性名，遍历对象的时候，该属性不会出现在`for...in`、`for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回。

由于以 Symbol 值作为键名，不会被常规方法遍历得到。我们可以利用这个特性，为对象定义一些非私有的、但又希望只用于内部的方法。

##### set和map数据结构

###### set

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

这是一个能够**存储无重复值**的有序列表

`Set`本身是一个构造函数，用来生成 Set 数据结构

- add()：添加某个值，返回set本身
- delete()：删除某个值，返回一个布尔值，判断删除是否成功
- has()：返回一个布尔值，表示该值是否为set成员
- clear()：清除所有成员，没有返回值
- keys()：返回键名的遍历器
- values()：返回键值的遍历器
- entries()：返回键值对的遍历器
- forEach()：使用回调函数遍历每个成员

```js
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

`Set`函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化

```js
// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三
const set = new Set(document.querySelectorAll('div'));
set.size // 56

// 类似于
const set = new Set();
document
 .querySelectorAll('div')
 .forEach(div => set.add(div));
set.size // 56
```

去除数组重复成员的方法

```json
// 去除数组的重复成员
[...new Set(array)]
```

去除字符串里面的重复字符。

```js
[...new Set('ababbc')].join('')
// "abc"
```

###### WeakSet

WeakSet 结构与 Set 类似，也是不重复的值的集合

WeakSet 的成员只能是对象，而不能是其他类型的值

WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中

WeakSet 是一个构造函数，可以使用`new`命令，创建 WeakSet 数据结构

WeakSet 可以接受一个数组或类似数组的对象作为参数。（实际上，任何具有 Iterable 接口的对象，都可以作为 WeakSet 的参数。）该数组的所有成员，都会自动成为 WeakSet 实例对象的成员。

```js
const a = [[1, 2], [3, 4]];
const ws = new WeakSet(a);
// WeakSet {[1, 2], [3, 4]}
```

WeakSet 不能遍历，是因为成员都是弱引用，随时可能消失，遍历机制无法保证成员的存在，很可能刚刚遍历结束，成员就取不到了。WeakSet 的一个用处，是储存 DOM 节点，而不用担心这些节点从文档移除时，会引发内存泄漏。

###### Map

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。

ES6中提供了Map数据结构，能够存放键值对，其中，键的去重是通过Object.is()方法进行比较，键的数据类型可以是基本类型数据也可以是对象，而值也可以是任意类型数据。

Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

事实上，不仅仅是数组，任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构（详见《Iterator》一章）都可以当作`Map`构造函数的参数。这就是说，`Set`和`Map`都可以用来生成新的 Map。

```js
 let map = new Map();
 map.set('title','hello world');
 map.set('year','2018');
 
 console.log(map.size); //2
```



##### set weakSet map weakMap 总结

Set 是无重复值的有序列表。根据 `Object.is()`方法来判断其中的值不相等，以保证无重复。 Set 会自动移除重复的值，因此你可以使用它来过滤数组中的重复值并返回结果。 Set并不是数组的子类型，所以你无法随机访问其中的值。但你可以使用`has()` 方法来判断某个值是否存在于 Set 中，或通过 `size` 属性来查看其中有多少个值。 Set 类型还拥有`forEach()`方法，用于处理每个值。

**Set 对象类似于数组，且成员的值都是唯一的**

Weak Set 是只能包含对象的特殊 Set 。其中的对象使用弱引用来存储，意味着当 Weak Set中的项是某个对象的仅存引用时，它不会屏蔽垃圾回收。由于内存管理的复杂性， Weak Set的内容不能被检查，因此最好将 Weak Set 仅用于追踪需要被归组在一起的对象。

Map 是有序的键值对，其中的键允许是任何类型。与 Set 相似，通过调用 `Object.is()`方法来判断重复的键，这意味着能将数值 5 与字符串 "5" 作为两个相对独立的键。使用`set()` 方法能将任何类型的值关联到某个键上，并且该值此后能用 `get()` 方法提取出来。Map 也拥有一个 `size` 属性与一个 `forEach()` 方法，让项目访问更容易。

**Map 对象是键值对集合，和 JSON 对象类似，但是 key 不仅可以是字符串还可以是对象**

Weak Map 是只能包含对象类型的键的特殊 Map 。与 Weak Set 相似，键的对象引用是弱引用，因此当它是某个对象的仅存引用时，也不会屏蔽垃圾回收。当键被回收之后，所关联的值也同时从 Weak Map 中被移除。

##### 应用场景

###### set

- 集合是一种**无序且唯一**的数据结构；
- `ES6` 中有集合，名为 `Set` ；
- **集合的常用操作：** 去重、判断某元素是否在集合中、求交集……

**模拟并集运算**

```js
const union = （setA， setB) => {
    const unionab = new Set()
    setA.forEach(value => unionab.add(value))
    setB.forEach(value => unionab.add(value))
    return [...unionab]
}

console.log(union([1, 2, 5, 8, 9], [4, 5, 8, 9, 10])) //[1, 2, 5, 8,9, 4, 10]
```

**模拟交集运算**

```js
const intersection = (setA, setB) => {
    const intersectionSet = new Set()
    const arrSetB = new Set(setB)
    setA.forEach(value => {
        if (arrSetB.has(value)) {
            intersectionSet.add(value)
        }
    })
    return [...intersectionSet]
}

console.log(intersection([1, 2, 5, 8, 9], [4, 5, 8, 9, 10])) //[5,8,9]
```

**模拟差集运算**

```js
const difference = (setA, setB) => {
    const differenceSet = new Set()
    const arrSetB = new Set(setB)
    setA.forEach(value => {
        if (!arrSetB.has(value)) {
            differenceSet.add(value)
        }
    })
    return [...differenceSet]
}

console.log(difference([1, 2, 5, 8, 9], [4, 5, 8, 9, 10])) //[1, 2]
```

**使用扩展运算符来模拟并集、交集和差集**

如果使用扩展运算符来进行运算的话，整个过程只需要三个步骤：

- 将集合转化为数组；
- 执行需要的运算；
- 将结果转化回集合。

**用扩展运算符实现并集**

```js
const union = (setA, setB) => {
    return new Set([...setA, ...setB]);
}
console.log(union([1, 2, 5, 8, 9], [4, 5, 8, 9, 10])) //[1, 2, 5, 8,9, 4, 10]
```

**用扩展运算符实现交集**

```js
const intersection = (setA, setB) => {
    const arrB = new Set(setB);
    return [...setA].filter(x => arrB.has(x));
}
console.log(intersection([1, 2, 5, 8, 9], [4, 5, 8, 9, 10])) //[5, 8, 9]

```

**用扩展运算符实现差集**

```js
const difference = (setA, setB) => {
    const arrB = new Set(setB)
   return [...setA].filter(x => !arrB.has(x));
}
console.log(difference([1, 2, 5, 8, 9], [4, 5, 8, 9, 10])) //[1, 2]

```

###### map

- 字典与**集合**相似，**字典**也是一种**存储唯一值**的数据结构，但它是以**键值对**的形式来存储。
- 注意：字典一定是以**键值对**的形式存储！！

**ES6中的Map可以做什么呢？**

- 使用 `Map` 对象： `new` 、 `set` 、 `delete` 、 `clear` ；
- 字典的常用操作，**键值对的增删改查**。

```js
const map = new Map()

//增
map.set('monday', '星期一')
map.set('Tuesday', '星期二')
map.set('Wednesday', '星期三')

console.log(map.has('monday')) //true
console.log(map.size) //3
console.log(map.keys()) //输出{'monday', 'Tuesday', 'Wednesday'}
console.log(map.values()) //输出{'星期一', '星期二', '星期三'}
console.log(map.get('monday')) //星期一

//删
map.delete('monday')

//清空
map.clear()

//改
map.set('monday', '星期四')
```



##### proxy

###### 定义

Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，

因此提供了一种机制，可以对外界的访问进行过滤和改写。

Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例

```js
var proxy = new Proxy(target, handler);

`new Proxy()`表示生成一个`Proxy`实例，`target`参数表示所要拦截的目标对象，`handler`参数也是一个对象，用来定制拦截行为
```

如果`handler`没有设置任何拦截，那就等同于直接通向原对象

```js
var target = {};
var handler = {};
var proxy = new Proxy(target, handler);
proxy.a = 'b';
target.a // "b"
```

###### Proxy 实例的方法

`get`方法用于拦截某个属性的读取操作，可以接受三个参数，依次为目标对象、属性名和 proxy 实例本身（严格地说，是操作行为所针对的对象），其中最后一个参数可选。

```js
var person = {
  name: "张三"
};

var proxy = new Proxy(person, {
  get: function(target, propKey) {
    if (propKey in target) {
      return target[propKey];
    } else {
      throw new ReferenceError("Prop name \"" + propKey + "\" does not exist.");
    }
  }
});

proxy.name // "张三"
proxy.age // 抛出一个错误
```

`set`方法用来拦截某个属性的赋值操作，可以接受四个参数，依次为目标对象、属性名、属性值和 Proxy 实例本身，其中最后一个参数可选。

假定`Person`对象有一个`age`属性，该属性应该是一个不大于 200 的整数，那么可以使用`Proxy`保证`age`的属性值符合要求。

```js
let validator = {
  set: function(obj, prop, value) {
    if (prop === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError('The age is not an integer');
      }
      if (value > 200) {
        throw new RangeError('The age seems invalid');
      }
    }

    // 对于满足条件的 age 属性以及其他属性，直接保存
    obj[prop] = value;
    return true;
  }
};

let person = new Proxy({}, validator);

person.age = 100;

person.age // 100
person.age = 'young' // 报错
person.age = 300 // 报错
```

`deleteProperty`方法用于拦截`delete`操作，如果这个方法抛出错误或者返回`false`，当前属性就无法被`delete`命令删除。

```js
var handler = {
  deleteProperty (target, key) {
    invariant(key, 'delete');
    delete target[key];
    return true;
  }
};
function invariant (key, action) {
  if (key[0] === '_') {
    throw new Error(`Invalid attempt to ${action} private "${key}" property`);
  }
}

var target = { _prop: 'foo' };
var proxy = new Proxy(target, handler);
delete proxy._prop

上面代码中，deleteProperty方法拦截了delete操作符，删除第一个字符为下划线的属性会报错。
```

##### Reflect

###### 定义

`Reflect`对象与`Proxy`对象一样，也是 ES6 为了操作对象而提供的新 API

`Reflect`对象的设计目的有这样几个

（1）将`Object`对象的一些明显属于语言内部的方法（比如`Object.defineProperty`），放到`Reflect`对象上

从`Reflect`对象上可以拿到语言内部的方法。

（2）修改某些`Object`方法的返回结果，让其变得更合理。

比如，`Object.defineProperty(obj, name, desc)`在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回`false`。

```js
// 老写法
try {
  Object.defineProperty(target, property, attributes);
  // success
} catch (e) {
  // failure
}

// 新写法
if (Reflect.defineProperty(target, property, attributes)) {
  // success
} else {
  // failure
}
```

（3） 让`Object`操作都变成函数行为。某些`Object`操作是命令式，比如`name in obj`和`delete obj[name]`，而`Reflect.has(obj, name)`和`Reflect.deleteProperty(obj, name)`让它们变成了函数行为

```js
// 老写法
'assign' in Object // true

// 新写法
Reflect.has(Object, 'assign') // true
```

（4）`Reflect`对象的方法与`Proxy`对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。这就让`Proxy`对象可以方便地调用对应的`Reflect`方法，完成默认行为，作为修改行为的基础。也就是说，不管`Proxy`怎么修改默认行为，你总可以在`Reflect`上获取默认行为。

```js
var loggedObj = new Proxy(obj, {
  get(target, name) {
    console.log('get', target, name);
    return Reflect.get(target, name);
  },
  deleteProperty(target, name) {
    console.log('delete' + name);
    return Reflect.deleteProperty(target, name);
  },
  has(target, name) {
    console.log('has' + name);
    return Reflect.has(target, name);
  }
});
```

上面代码中，每一个`Proxy`对象的拦截操作（`get`、`delete`、`has`），内部都调用对应的`Reflect`方法，保证原生行为能够正常执行。添加的工作，就是将每一个操作输出一行日志。

###### 静态方法

`Reflect`对象一共有 13 个静态方法。

- Reflect.apply(target, thisArg, args)
- Reflect.construct(target, args)
- Reflect.get(target, name, receiver)
- Reflect.set(target, name, value, receiver)
- Reflect.defineProperty(target, name, desc)
- Reflect.deleteProperty(target, name)
- Reflect.has(target, name)
- Reflect.ownKeys(target)
- Reflect.isExtensible(target)
- Reflect.preventExtensions(target)
- Reflect.getOwnPropertyDescriptor(target, name)
- Reflect.getPrototypeOf(target)
- Reflect.setPrototypeOf(target, prototype)

`Reflect.get`方法查找并返回`target`对象的`name`属性，如果没有该属性，则返回`undefined`。

```js
var myObject = {
  foo: 1,
  bar: 2,
  get baz() {
    return this.foo + this.bar;
  },
}

Reflect.get(myObject, 'foo') // 1
Reflect.get(myObject, 'bar') // 2
Reflect.get(myObject, 'baz') // 3
```

`Reflect.set`方法设置`target`对象的`name`属性等于`value`。

```js
var myObject = {
  foo: 1,
  set bar(value) {
    return this.foo = value;
  },
}

myObject.foo // 1

Reflect.set(myObject, 'foo', 2);
myObject.foo // 2

Reflect.set(myObject, 'bar', 3)
myObject.foo // 3
```

`Reflect.has`方法对应`name in obj`里面的`in`运算符。

```js
var myObject = {
  foo: 1,
};

// 旧写法
'foo' in myObject // true

// 新写法
Reflect.has(myObject, 'foo') // true
```

##### promise

###### 定义

Promise 是异步编程的一种解决方案，同时解决了回调地狱的问题

Promise是一个构造函数，自己身上有all、reject、resolve这几个方法，原型上有then、catch等方法
Promise的构造函数接收一个函数作为参数，并且这个函数传入两个参数：resolve，reject，分别表示异步操作执行成功后和失败后的回调函数

```js
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

`resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），

在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

`reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），

在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

`Promise`实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数。

`Promise`对象有以下两个特点。

（1）对象的状态不受外界影响。

`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。

`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

`Promise`也有一些缺点。首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。

其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部

###### Promise.prototype.then()

`then`方法是定义在原型对象`Promise.prototype`上的，它的作用是为 Promise 实例添加状态改变时的回调函数

`then`方法的第一个参数是`resolved`状态的回调函数，第二个参数是`rejected`状态的回调函数，它们都是可选的
Promise.prototype.then(onResolve（）{}，onReject（）{})

`then`方法返回的是一个新的`Promise`实例（注意，不是原来那个`Promise`实例）。因此可以采用链式写法，即`then`方法后面再调用另一个`then`方法。

###### Promise.prototype.catch()

Promise.prototype.catch(onReject（）{})，返回一个新的promise

`Promise.prototype.catch()`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数，这个方法只接收一个参数。事实上，这个方法就是一个语法糖，调用它就相当于调用 Promise.prototype. then(null, onRejected)

Promise 对象状态变为`resolved`，则会调用`then()`方法指定的回调函数；如果异步操作抛出错误，状态就会变为`rejected`，就会调用`catch()`方法指定的回调函数，处理这个错误。另外，`then()`方法指定的回调函数，如果运行中抛出错误，也会被`catch()`方法捕获

###### Promise.prototype.finally()

`finally()`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作

###### Promise.all()

用于将多个 Promise 实例，包装成一个新的 Promise 实例

`Promise.all()`方法接受一个数组作为参数，`p1`、`p2`、`p3`都是 Promise 实例

```js
const p = Promise.all([p1, p2, p3]);
```

`p`的状态由`p1`、`p2`、`p3`决定，分成两种情况。

（1）只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。

（2）只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

###### Promise.race()

只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数

```js
const p = Promise.race([p1, p2, p3]);
```

###### Promise.allSettled()

确定一组异步操作是否都结束了（不管成功或失败）。

所以，它的名字叫做”Settled“，包含了”fulfilled“和”rejected“两种情况。

`Promise.allSettled()`方法接受一个数组作为参数，数组的每个成员都是一个 Promise 对象，并返回一个新的 Promise 对象。

只有等到参数数组的所有 Promise 对象都发生状态变更（不管是`fulfilled`还是`rejected`），返回的 Promise 对象才会发生状态变更。

###### Promise.any()

该方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例返回。

只要参数实例有一个变成`fulfilled`状态，包装实例就会变成`fulfilled`状态；如果所有参数实例都变成`rejected`状态，包装实例就会变成`rejected`状态。

`Promise.any()`跟`Promise.race()`方法很像，只有一点不同，就是`Promise.any()`不会因为某个 Promise 变成`rejected`状态而结束，必须等到所有参数 Promise 变成`rejected`状态才会结束。

###### Promise.resolve()

现有对象转为 Promise 对象

```js
const jsPromise = Promise.resolve($.ajax('/whatever.json'));
```

###### Promise.reject()

`Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected`。

###### 手写promise

```js
class Undertake {
    // promise有三种状态机，需要手动改变状态。
    // 根据构造函数时传递的参数去改变状态
    static PENDING = 'pending';
    static FULFILLED = 'fulfilled';
    static REJECT = 'reject';
    constructor(excuteor) {
        this.status = Undertake.PENDING;
        this.value = null;
        try {
            excuteor(this.resolve.bind(this), this.reject.bind(this))
        } catch (error) {
            this.reject(error)
        }

    }
    resolve(value) {
        if (this.status === Undertake.PENDING) {
            this.status = Undertake.FULFILLED;
            this.value = value;
            console.log(this.value);
        }
    }
    reject(value) {
        if (this.status === Undertake.PENDING) {
            this.status = Undertake.REJECT;
            this.value = value;
        }
    }
    then(onFulfilled, onReject) {
        if(typeof onFulfilled!='function'){
            onFulfilled=()=>{}
        }
        if(typeof onReject!='function'){
            onReject=()=>{}
        }
        if (this.status === Undertake.FULFILLED) {
            onFulfilled(this.value)
        }
        if (this.status === Undertake.REJECT) {
            onReject(this.value)
        }
    }
}
// new Promise((resolve,reject)=>{})
```



##### Iterator 和 for...of 循环

###### 起源

JavaScript 原有的表示“集合”的数据结构，主要是数组（`Array`）和对象（`Object`），ES6 又添加了`Map`和`Set`。这样就有了四种数据集合，用户还可以组合使用它们，定义自己的数据结构，比如数组的成员是`set`，`Map`的成员是对象。这样就需要一种统一的接口机制，来处理所有不同的数据结构。

遍历器（Iterator）就是这样一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

迭代器是一个对象，这个对象允许对它的值集合进行遍历，并保持任何必要的状态以便能够跟踪到当前遍历的位置

Iterator 的作用有三个：

一是为各种数据结构，提供一个统一的、简便的访问接口；

二是使得数据结构的成员能够按某种次序排列；

三是 ES6 创造了一种新的遍历命令`for...of`循环，Iterator 接口主要供`for...of`消费。

###### 默认 Iterator 接口

Iterator 接口的目的，就是为所有数据结构，提供了一种统一的访问机制，即`for...of`循环（详见下文）。

当使用`for...of`循环遍历某种数据结构时，该循环会自动去寻找 Iterator 接口。

一种数据结构只要部署了 Iterator 接口，我们就称这种数据结构是“可遍历的”（iterable）。

###### 原生具备 Iterator 接口的数据结构

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象

###### for...of 循环

ES6 借鉴 C++、Java、C# 和 Python 语言，引入了`for...of`循环，作为遍历所有数据结构的统一的方法。

一个数据结构只要部署了`Symbol.iterator`属性，就被视为具有 iterator 接口，就可以用`for...of`循环遍历它的成员。也就是说，`for...of`循环内部调用的是数据结构的`Symbol.iterator`方法。

`for...of`循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如`arguments`对象、DOM NodeList 对象）、后文的 Generator 对象，以及字符串。

对于普通的对象，`for...of`结构不能直接使用，会报错，必须部署了 Iterator 接口后才能使用

**为什么对象（Object）没有部署Iterator接口呢？**

有两个原因：一是因为对象的哪个属性先遍历，哪个属性后遍历是不确定的，需要开发者手动指定。然而遍历遍历器是一种线性处理，对于非线性的数据结构，部署遍历器接口，就等于要部署一种线性转换。二是对对象部署Iterator接口并不是很必要，因为Map弥补了它的缺陷，又正好有Iteraotr接口

###### 与其他遍历方法的比较

**区别**

for … of遍历获取的是对象的键值,for … in 获取的是对象的键名

for … in会遍历对象的整个原型链,性能非常差不推荐使用,而for … of只遍历当前对象不会遍历原型链

对于数组的遍历,for … in会返回数组中所有可枚举的属性(包括原型链上可枚举的属性),for … of只返回数组的下标对应的属性值

for…in循环有几个缺点
　　①数组的键名是数字，但是for…in循环是以字符串作为键名“0”、“1”、“2”等等。
　　②for…in循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键。
　　③某些情况下，for…in循环会以任意顺序遍历键名。
　　for…in循环主要是为遍历对象而设计的，不适用于遍历数组。

for…of循环
　　有着同for…in一样的简洁语法，但是没有for…in那些缺点。
　　不同于forEach方法，它可以与break、continue和return配合使用。
　　提供了遍历所有数据结构的统一操作接口
		for...of 循环可以用来遍历数组、类数组对象，字符串、Set、Map 以及 Generator 对象

##### class类

ES6 的`class`可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的`class`写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已

```js
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

###### constructor 方法

`constructor()`方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。

一个类必须有`constructor()`方法，如果没有显式定义，一个空的`constructor()`方法会被默认添加。

###### 类的实例

与 ES5 一样，实例的属性除非显式定义在其本身（即定义在`this`对象上），否则都是定义在原型上（即定义在`class`上）。

```js
//定义类
class Point {

  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }

}

var point = new Point(2, 3);

point.toString() // (2, 3)

point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
point.__proto__.hasOwnProperty('toString') // true
```

上面代码中，`x`和`y`都是实例对象`point`自身的属性（因为定义在`this`对象上），所以`hasOwnProperty()`方法返回`true`，而`toString()`是原型对象的属性（因为定义在`Point`类上），所以`hasOwnProperty()`方法返回`false`。这些都与 ES5 的行为保持一致。

###### 取值函数（getter）和存值函数（setter）

与 ES5 一样，在“类”的内部可以使用`get`和`set`关键字，

对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```js
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```

###### 属性表达式

类的属性名，可以采用表达式

```js
let methodName = 'getArea';

class Square {
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
```

###### **this 的指向**

类的方法内部如果含有`this`，它默认指向类的实例

###### 静态方法

如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```js
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

###### 实例属性的新写法

实例属性除了定义在`constructor()`方法里面的`this`上面，也可以定义在类的最顶层。

##### module语法

###### 概述

在 ES6 之前，社区制定了一些模块加载方案，最主要的有 CommonJS 和 AMD 两种。

前者用于服务器，后者用于浏览器

ES6 在语言标准的层面上，实现了模块功能

###### export 命令

模块功能主要由两个命令构成：`export`和`import`。

`export`命令用于规定模块的对外接口，`import`命令用于输入其他模块提供的功能。

###### 模块的整体加载

除了指定加载某个输出值，还可以使用整体加载，即用星号（`*`）指定一个对象，所有输出值都加载在这个对象上面

```js
//逐一加载
import { area, circumference } from './circle';
//全部加载
import * as circle from './circle'

```

###### export default

为模块指定默认输出

```js
// export-default.js
export default function foo() {
  console.log('foo');
}

// 或者写成

function foo() {
  console.log('foo');
}

export default foo;

import crc32 from 'crc32'
```

使用`export default`时，对应的`import`语句不需要使用大括号；

不使用`export default`时，对应的`import`语句需要使用大括号

###### import()

（1）按需加载。

`import()`可以在需要的时候，再加载某个模块。

```js
button.addEventListener('click', event => {
  import('./dialogBox.js')
  .then(dialogBox => {
    dialogBox.open();
  })
  .catch(error => {
    /* Error handling */
  })
});
```

（2）条件加载

`import()`可以放在`if`代码块，根据不同的情况，加载不同的模块。

```js
if (condition) {
  import('moduleA').then(...);
} else {
  import('moduleB').then(...);
}
```

（3）动态的模块路径

`import()`允许模块路径动态生成。

```js
import(f())
.then(...);
```

##### 可选链操作符

```js
const name = obj?.name;
```

