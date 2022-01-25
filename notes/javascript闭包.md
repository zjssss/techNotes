##### 描述

闭包（closure）是Javascript语言的一个难点，也是它的特色，很多高级应用都要依靠闭包实现

##### 变量作用域

变量的作用域无非就是两种：全局变量和局部变量

函数内部可以直接读取全局变量

```js
var n=999;

　　function f1(){
　　　　alert(n);
　　}

　　f1(); // 999
```

在函数外部自然无法读取函数内的局部变量

```js
　　function f1(){
　　　　var n=999;
　　}

　　alert(n); // error
```

##### **如何从外部读取局部变量**

正常情况下，我们是做不到在函数外面获取函数里面的变量。只有通过变通方法才能实现，

那就是在函数的内部，再定义一个函数

```js
function f1(){

　　　　var n=999;

　　　　function f2(){
　　　　　　alert(n); // 999
　　　　}

　　}
```

在上面的代码中，函数f2就被包括在函数f1内部，这时f1内部的所有局部变量，对f2都是可见的。f2可以访问f1函数的变量。
但是反过来就不行，f2内部的局部变量，对f1就是不可见的。f1不能访问f2的变量。
这就是Javascript语言特有的"链式作用域"结构（chain scope），子对象会一级一级地向上寻找所有父对象的变量。
所以，父对象的所有变量，对子对象都是可见的，反之则不成立。

既然f2可以读取f1中的局部变量，那么只要把f2作为返回值，就可以在f1外部读取它的内部变量

```js
function f1(){

　　　　var n=999;

　　　　function f2(){
　　　　　　alert(n);
　　　　}

　　　　return f2;

　　}

　　var result=f1();

　　result(); // 999
```

##### 闭包的概念

其实闭包的概念是比较抽象的，在我的理解看来闭包就是能够读取其他函数内部变量的函数。

是函数嵌套着另一个函数形成了闭包
比如：函数 a 内嵌套 b,且返回 b,当调用函数 a 时,用变量接收函数 b,就形成了闭包

在一个外函数中定义了一个内函数，内函数里运用了外函数的临时变量或者参数，并且外函数的返回值是内函数的引用。这样就构成了一个闭包

**闭包的最大用处有两个，一个是可以读取函数内部的变量，另一个就是让这些变量始终保持在内存中，即闭包可以使得它诞生环境一直存在，延长变量的作用域链**

```js
function makeSizer(size) {
  return function() {
    document.body.style.fontSize = size + 'px';
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);

document.getElementById('size-12').onclick = size12;
document.getElementById('size-14').onclick = size14;
document.getElementById('size-16').onclick = size16;
```



##### 闭包的3个特性

①函数嵌套函数
②函数内部可以引用函数外部的参数和变量
③参数和变量不会被垃圾回收机制回收

##### 闭包的用途

闭包可以用在许多地方。它的最大用处有两个，

一个是前面提到的可以读取函数内部的变量，缓存数据，延长变量的生命周期

另一个就是让这些变量的值始终保持在内存中，方便我们读取数据。

##### 优点

1.可以读取函数内部的变量
2.可以避免全局污染

##### 缺点

1.闭包会导致变量不会被垃圾回收机制所清除，会大量消耗内存；

2.不恰当的使用闭包可能会造成内存泄漏的问题；

##### **使用闭包的注意点**

1）由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。

解决方法是，在退出函数之前，将不使用的局部变量全部删除。

2）闭包会在父函数外部，改变父函数内部变量的值。

所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

##### 使用场景

###### setTimeout

原生的setTimeout传递的第一个函数不能带参数，通过闭包可以实现传参效果

```js
function f1(a) {
    function f2() {
        console.log(a);
    }
    return f2;
}
var fun = f1(1);
setTimeout(fun,1000);//一秒之后打印出1
```

###### 回调函数

定义行为，然后把它关联到某个用户事件上（点击或者按键）。代码通常会作为一个回调（事件触发时调用的函数）绑定到事件

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>测试</title>
</head>
<body>
    <a href="#" id="size-12">12</a>
    <a href="#" id="size-20">20</a>
    <a href="#" id="size-30">30</a>

    <script type="text/javascript">
        function changeSize(size){
            return function(){
                document.body.style.fontSize = size + 'px';
            };
        }

        var size12 = changeSize(12);
        var size14 = changeSize(20);
        var size16 = changeSize(30);

        document.getElementById('size-12').onclick = size12;
        document.getElementById('size-20').onclick = size14;
        document.getElementById('size-30').onclick = size16;

    </script>
</body>
</html>
```

###### 函数防抖

在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时

实现的关键就在于`setTimeOut`这个函数，由于还需要一个变量来保存计时，考虑维护全局纯净，可以借助闭包来实现

```js
/*
* fn [function] 需要防抖的函数
* delay [number] 毫秒，防抖期限值
*/
function debounce(fn,delay){
    let timer = null
    //借助闭包
    return function() {
        if(timer){
            clearTimeout(timer) //进入该分支语句，说明当前正在一个计时过程中，并且又触发了相同事件。所以要取消当前的计时，重新开始计时
            timer = setTimeOut(fn,delay) 
        }else{
            timer = setTimeOut(fn,delay) // 进入该分支说明当前并没有在计时，那么就开始一个计时
        }
    }
}
```

###### 封装私有变量

用js创建一个计数器

```js
function f1() {
    var sum = 0;
    var obj = {
       inc:function () {
           sum++;
           return sum;
       }
};
    return obj;
}
let result = f1();
console.log(result.inc());//1
console.log(result.inc());//2
console.log(result.inc());//3
```

