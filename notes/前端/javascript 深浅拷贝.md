#### 定义

深拷贝和浅拷贝只针对像 Object, Array 这样的复杂对象的。浅拷贝只复制一层对象的属性，而深拷贝则递归复制了所有层级。

#### 为什么要使用深拷贝

我们希望在改变新的数组（对象）的时候，不改变原数组（对象）

#### 深拷贝的要求程度

我们在使用深拷贝的时候，一定要弄清楚我们对深拷贝的要求程度：是仅“深”拷贝第一层级的对象属性或数组元素，还是递归拷贝所有层级的对象属性和数组元素？

#### 怎么检验深拷贝成功

改变任意一个新对象/数组中的属性/元素, 都不改变原对象/数组

#### 浅拷贝（只能拷贝第一层级）

##### 进行遍历

```js
var array = [1, 2, 3, 4];
function copy (array) {
   let newArray = []
   for(let item of array) {
      newArray.push(item);
   }
   return  newArray;
}
var copyArray = copy(array);
copyArray[0] = 100;
console.log(array); // [1, 2, 3, 4]
console.log(copyArray); // [100, 2, 3, 4]
```



##### ES6的Object.assign()

```
var o2 = Object.assign({}, o1)
```

#####  ES6扩展运算符

```
var o2 = {...o1}
```

##### 数组 Array.from()

```
let items2 = Array.from(items1)
```

##### 数组的slice，concat

```
var copyArray = array.slice();
```



#### 深拷贝（拷贝所有层级）

#####  JSON.parse(JSON.string()) 

可满足大多数情况，除了对象中含有function

```js
var array = [
    { number: 1 },
    { number: 2 },
    { number: 3 }
];
var copyArray = JSON.parse(JSON.stringify(array))
copyArray[0].number = 100;
console.log(array); //  [{number: 1}, { number: 2 }, { number: 3 }]
console.log(copyArray); // [{number: 100}, { number: 2 }, { number: 3 }]
```

#####  手动写递归函数

可满足所有情况

```js
function deepClone(obj){
  let clone = Array.isArray(obj)?[]:{};
  for (const key in obj) {
      let item = obj[key];
      if(item){
          //实现方法的克隆
          if(item instanceof Function){
              clone[key] = new Function('return '+item.toString())()
          }
          else if(item instanceof Object ){
              clone[key] = deepClone(item);
          }
          else {
              clone[key] = item;
          }
      }
  }
  return clone;
}
```

##### 3使用lodash.js的cloneDeep api



#### 关于直接赋值

var obj1 = obj;仅仅是指向同一个地址，并不是浅拷贝，浅拷贝后的值类型数值是互不影响的，

所以直接赋值并不属于深浅拷贝这一列。