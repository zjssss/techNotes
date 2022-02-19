### 对象

在JS中，万物皆对象，对象又分为普通对象和函数对象
其中 Object、Function 为 JS 自带的函数对象

```js
let obj1 = {}; 
let obj2 = new Object();
let obj3 = new fun1()

function fun1(){}; 
let fun2 = function(){};
let fun3 = new Function('some','console.log(some)');

// JS自带的函数对象
console.log(typeof Object); //function 
console.log(typeof Function); //function  

// 普通对象
console.log(typeof obj1); //object 
console.log(typeof obj2); //object 
console.log(typeof obj3); //object

// 函数对象
console.log(typeof fun1); //function 
console.log(typeof fun2); //function 
console.log(typeof fun3); //function   
```



### 构造函数

构造函数也是对象的一种

与大部分面向对象语言不同，ES6之前中并没有引入类（class）的概念，JavaScript并非通过类而是直接通过构造函数来创建实例。

```js
 function Person(name, age, gender) {
        this.name = name
        this.age = age
        this.gender = gender
        this.sayName = function () {
            alert(this.name);
        }
    }
    var per = new Person("孙悟空", 18, "男");
    function Dog(name, age, gender) {
        this.name = name
        this.age = age
        this.gender = gender
    }
    var dog = new Dog("旺财", 4, "雄")
    console.log(per);//当我们直接在页面中打印一个对象时，事件上是输出的对象的toString()方法的返回值
    console.log(dog);
```

问题：每创建一个Person构造函数，在Person构造函数中，为每一个对象都添加了一个sayName方法，也就是说构造函数每执行一次就会创建一个新的sayName方法。这样就导致了构造函数执行一次就会创建一个新的方法，执行10000次就会创建10000个新的方法，而10000个方法都是一摸一样的，为什么不把这个方法单独放到一个地方，并让所有的实例都可以访问到呢?这就需要原型(`prototype`)

### 原型的三角关系

用来初始化新创建的对象的函数是构造函数。在例子中，Foo()函数是构造函数

```js
function Foo(){}
var foo = new Foo()；
```

通过构造函数的new操作创建的对象是实例对象。可以用一个构造函数，构造多个实例对象

```js
function Foo(){};
var f1 = new Foo;
var f2 = new Foo;
console.log(f1 === f2);//false
```

#### 原型对象及prototype

构造函数有一个prototype属性，这个属性是一个指针，指向该构造函数的原型对象。这个对象包含了通过调用该构造函数所创建的对象共享的属性和方法
通过同一个构造函数实例化的多个对象具有相同的原型对象。经常使用原型对象来实现继承

```js
function Foo(){};
Foo.prototype.a = 1;
var f1 = new Foo;
var f2 = new Foo;

console.log(Foo.prototype.a);//1
console.log(f1.a);//1
console.log(f2.a);//1
```

#### constructor

原型对象有一个constructor属性，指向该原型对象对应的构造函数

```js
function Foo(){};
console.log(Foo.prototype.constructor === Foo);//true
```

由于实例对象可以继承原型对象的属性，所以实例对象也拥有constructor属性，同样指向原型对象对应的构造函数

```js
function Foo(){};
var f1 = new Foo;
console.log(f1.constructor === Foo);//true
```

#### proto

实例对象有一个proto属性，指向该实例对象对应的原型对象，也就是说实例对象的proto指向了该构造函数的prototype

```js
function Foo(){};
var f1 = new Foo;
console.log(f1.__proto__ === Foo.prototype);//true
```

#### 详细说明

实例对象f1是通过构造函数Foo()的new操作创建的。构造函数Foo()的原型对象是Foo.prototype；实例对象f1通过__proto__属性也指向原型对象Foo.prototype

```js
function Foo(){};
var f1 = new Foo;
console.log(f1.__proto === Foo.prototype);//true
```

实例对象f1本身并没有constructor属性，但它可以继承原型对象Foo.prototype的constructor属性

```js
function Foo(){};
var f1 = new Foo;
console.log(Foo.prototype.constructor === Foo);//true
console.log(f1.constructor === Foo);//true
console.log(f1.hasOwnProperty('constructor'));//false
```

Foo.prototype是f1的原型对象，同时它也是实例对象。实际上，任何对象都可以看做是通过Object()构造函数的new操作实例化的对象

　　所以，Foo.prototype作为实例对象，它的构造函数是Object()，原型对象是Object.prototype。相应地，构造函数Object()的prototype属性指向原型对象Object.prototype；实例对象Foo.prototype的proto属性同样指向原型对象Object.prototype

```js
function Foo(){};
var f1 = new Foo;
console.log(Foo.prototype.__proto__ === Object.prototype);//true
```

实例对象Foo.prototype本身具有constructor属性，所以它会覆盖继承自原型对象Object.prototype的constructor属性

```js
function Foo(){};
var f1 = new Foo;
console.log(Foo.prototype.constructor === Foo);//true
console.log(Object.prototype.constructor === Object);//true
console.log(Foo.prototype.hasOwnProperty('constructor'));//true
```

#### 原型链

当我们访问对象的一个属性或方法时，它会先在对象自身中寻找，如果有则直接使用，如果没有则会去原型对象中寻找，如果找到则直接使用。如果没有则去原型的原型中寻找,直到找到Object对象的原型，Object对象的原型没有原型，如果在Object原型中依然没有找到，则返回null。

Object是JS中所有对象数据类型的基类(最顶层的类)在Object.prototype上没有`__proto__`这个属性。

```js
console.log(Object.prototype.__proto__ === null) // true
```

### new实现的过程

当我们new一个构造函数，得到的实例继承了**构造器的构造属性**(this.name这些)以及**原型上的属性**。

创建一个空对象
将它的引用赋给 this，继承函数的原型，也就是this指针指向了当前对象。
通过 this 将属性和方法添加至这个对象
最后把this返回出去，也就是把当前对象返回出去

```js
// ES5构造函数
let Parent = function (name, age) {
    //1.创建一个新对象，赋予this，这一步是隐性的，
    // let this = {};
    //2.给this指向的对象赋予构造属性
    this.name = name;
    this.age = age;
    //3.如果没有手动返回对象，则默认返回this指向的这个对象，也是隐性的
    // return this;
};
const child = new Parent();
const child2 = new Parent();
console.log(child===child2)//false
```



### 继承的种类

原型链继承、借用构造函数继承、组合继承、寄生组合继承、原型式继承、寄生式继承、 ES6 继承

### ES5的继承

先定义一个父类

```js
function SuperType () {
  // 属性
  this.name = 'SuperType';
}
// 原型方法
SuperType.prototype.sayName = function() {
  return this.name;
};
```

#### 原型链继承

##### 基本思想

将父类的实例作为子类的原型

```js
// 父类
function SuperType () {
  this.name = 'SuperType'; // 父类属性
}
SuperType.prototype.sayName = function () { // 父类原型方法
  return this.name;
};

// 子类
function SubType () {
  this.subName = "SubType"; // 子类属性
};

SubType.prototype = new SuperType(); // 重写原型对象，代之以一个新类型的实例
// 这里实例化一个 SuperType 时， 实际上执行了两步
// 1，新创建的对象复制了父类构造函数内的所有属性及方法
// 2，并将原型 __proto__ 指向了父类的原型对象

SubType.prototype.saySubName = function () { // 子类原型方法
  return this.subName;
}

// 子类实例
let instance = new SubType();

// instanceof 通过判断对象的 prototype 链来确定对象是否是某个类的实例
instance instanceof SubType; // true
instance instanceof SuperType; // true

// 注意
SubType instanceof SuperType; // false
SubType.prototype instanceof SuperType ; // true
```

##### 特点

利用原型，让一个引用类型继承另一个引用类型的属性及方法

##### 优点

继承了父类的模板，又继承了父类的原型对象

##### 缺点

可以在子类构造函数中，为子类实例增加实例属性。如果要**新增原型属性和方法**，则必须放在 `SubType.prototype = new SuperType('SubType');` 这样的语句**之后**执行

无法实现多继承

来自原型对象的所有属性被**所有实例共享**，更改原型属性会影响所有实例

```js
// 父类
function SuperType () {
  this.colors = ["red", "blue", "green"];
  this.name = "SuperType";
}
// 子类
function SubType () {}

// 原型链继承
SubType.prototype = new SuperType();

// 实例1
var instance1 = new SubType();
instance1.colors.push("blcak");
instance1.name = "change-super-type-name";
console.log(instance1.colors); // ["red", "blue", "green", "blcak"]
console.log(instance1.name); // change-super-type-name
// 实例2
var instance2 = new SubType();
console.log(instance2.colors); // ["red", "blue", "green", "blcak"]
console.log(instance2.name); // SuperType
```

- **注意**：更改 `SuperType` **引用类型属性**时，会使 `SubType` 所有实例共享这一更新。基础类型属性更新则不会。
- **创建子类实例时，无法向父类构造函数传参**，或者说是，没办法在不影响所有对象实例的情况下，向超类的构造函数传递参数

#### 借用构造函数继承

##### 基本思想

在子类的构造函数内部使用 apply/call 调用父类型构造函数

**注意：**

- 函数只不过是在特定环境中执行代码的对象，所以这里使用 apply/call 来实现。
- 使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）

```js
// 父类
function SuperType (name) {
  this.name = name; // 父类属性
}
SuperType.prototype.sayName = function () { // 父类原型方法
  return this.name;
};

// 子类
function SubType () {
  // 调用 SuperType 构造函数
  SuperType.call(this, 'SuperType'); // 在子类构造函数中，向父类构造函数传参
  // 为了保证子父类的构造函数不会重写子类的属性，需要在调用父类构造函数后，定义子类的属性
  this.subName = "SubType"; // 子类属性
};
// 子类实例
let instance = new SubType(); // 运行子类构造函数，并在子类构造函数中运行父类构造函数，this绑定到子类
```

##### 优点

解决了原型链继承中子类实例共享父类引用对象的问题，实现多继承，创建子类实例时，可以向父类传递参数

##### 缺点

- 实例并不是父类的实例，只是子类的实例
- **只能**继承父类的实例属性和方法，**不能**继承原型属性/方法
- **无法实现函数复用**，每个子类都有父类实例函数的副本，影响性能

#### 组合继承

组合继承就是将原型链继承与构造函数继承组合在一起，从而发挥两者之长的一种继承模式。

##### 基本思想

通过原型链继承实现对原型属性和方法的继承，通过构造函数继承来实现对实例属性的继承。
这样既能通过在原型上定义方法实现函数复用，又能保证每个实例都有自己的属性。

通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用

```js
// 父类
function SuperType (name) {
  this.colors = ["red", "blue", "green"];
  this.name = name; // 父类属性
}
SuperType.prototype.sayName = function () { // 父类原型方法
  return this.name;
};

// 子类
function SubType (name, subName) {
  // 调用 SuperType 构造函数
  SuperType.call(this, name); // ----第二次调用 SuperType----
  this.subName = subName;
};

// ----第一次调用 SuperType----
SubType.prototype = new SuperType(); // 重写原型对象，代之以一个新类型的实例

SubType.prototype.constructor = SubType; // 组合继承需要修复构造函数指向
SubType.prototype.saySubName = function () { // 子类原型方法
  return this.subName;
}

// 子类实例
let instance = new SubType('An', 'sisterAn')
instance.colors.push('black')
console.log(instance.colors) // ["red", "blue", "green", "black"]
instance.sayName() // An
instance.saySubName() // sisterAn

let instance1 = new SubType('An1', 'sisterAn1')
console.log(instance1.colors) //  ["red", "blue", "green"]
instance1.sayName() // An1
instance1.saySubName() // sisterAn1
```

##### 优点

1、可以继承父类原型上的属性，可以传参，可复用。
2、每个新实例引入的构造函数属性是私有的。

##### 缺点

调用了两次父类构造函数（耗内存），子类的构造函数会代替原型上的那个父类构造函数

#### 寄生组合继承

寄生式继承和组合式继承的结合版

在组合继承中，调用了两次父类构造函数，这里 通过通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点

##### 基本思想

借用构造函数继承属性 ，通过原型链的混成形式来继承方法

```js
// 父类
function SuperType (name) {
  this.colors = ["red", "blue", "green"];
  this.name = name; // 父类属性
}
SuperType.prototype.sayName = function () { // 父类原型方法
  return this.name;
};

// 子类
function SubType (name, subName) {
  // 调用 SuperType 构造函数
  SuperType.call(this, name); // ----第二次调用 SuperType，继承实例属性----
  this.subName = subName;
};

// ----第一次调用 SuperType，继承原型属性----
SubType.prototype = Object.create(SuperType.prototype)

SubType.prototype.constructor = SubType; // 注意：增强对象

let instance = new SubType('An', 'sisterAn')
```

##### 思路

不必为了指定子类的原型而调用父类的构造函数,我们所需要的无非就是父类原型的一个副本而已.本质上,就是使用寄生式继承来继承父类的原型,然后在将结果指定给子类的原型

##### 优点

- 只调用一次 `SuperType` 构造函数，只创建一份父类属性
- 原型链保持不变
- 能够正常使用 `instanceof` 与 `isPrototypeOf`

##### Object.create()

Object.create()方法是ES5中原型式继承的规范化

在只传一个参数时，内部行为如下：

```js
function object (o) {
	function F() {};
	F.prototype = o;
	return new F();
}
```



#### 原型式继承

实现思路就是将子类的原型设置为父类的原型

```js
// 父类
function SuperType (name) {
  this.colors = ["red", "blue", "green"];
  this.name = name; // 父类属性
}
SuperType.prototype.sayName = function () { // 父类原型方法
  return this.name;
};

/** 第一步 */
// 子类，通过 call 继承父类的实例属性和方法，不能继承原型属性/方法
function SubType (name, subName) {
  SuperType.call(this, name); // 调用 SuperType 的构造函数，并向其传参 
  this.subName = subName;
}

/** 第二步 */
// 解决 call 无法继承父类原型属性/方法的问题
// Object.create 方法接受传入一个作为新创建对象的原型的对象，创建一个拥有指定原型和若干个指定属性的对象
// 通过这种方法指定的任何属性都会覆盖原型对象上的同名属性
SubType.prototype = Object.create(SuperType.prototype, { 
  constructor: { // 注意指定 SubType.prototype.constructor = SubType
    value: SubType,
    enumerable: false,
    writable: true,
    configurable: true
  },
  run : { 
    value: function(){ // override
      SuperType.prototype.run.apply(this, arguments); 
      	// call super
      	// ...
    },
    enumerable: true,
    configurable: true, 
    writable: true
  }
}) 

/** 第三步 */
// 最后：解决 SubType.prototype.constructor === SuperType 的问题
// 这里，在上一步已经指定，这里不需要再操作
// SubType.prototype.constructor = SubType;

var instance = new SubType('An', 'sistenAn')
```

### ES6继承

es6的继承就相当于寄生组合继承

首先，实现一个简单的 ES6 继承

```js
class People {
    constructor(name) {
        this.name = name
    }
    run() { }
}

// extends 相当于方法的继承
// 替换了上面的3行代码
class Man extends People {
    constructor(name) {
        // super 相当于属性的继承
        // 替换了 People.call(this, name)
        super(name)
        this.gender = '男'
    }
    fight() { }
}
```

#### 核心

`extends` 继承的核心代码如下，其实现和上述的寄生组合式继承方式一样

```js
function _inherits(subType, superType) {
    // 创建对象，Object.create 创建父类原型的一个副本
    // 增强对象，弥补因重写原型而失去的默认的 constructor 属性
    // 指定对象，将新创建的对象赋值给子类的原型 subType.prototype
    subType.prototype = Object.create(superType && superType.prototype, {
        constructor: { // 重写 constructor
            value: subType,
            enumerable: false,
            writable: true,
            configurable: true
        }
    });
    if (superType) {
        Object.setPrototypeOf 
            ? Object.setPrototypeOf(subType, superType) 
            : subType.__proto__ = superType;
    }
}
```

### 继承的使用场景

- 不要仅仅为了使用而使用它们，这只是在浪费时间而已。
- 当需要创建 **一系列拥有相似特性的对象** 时，那么创建一个包含所有共有功能的通用对象，然后在更特殊的对象类型中继承这些特性。
- 应避免多继承，造成混乱。