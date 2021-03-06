## 查询属性兼容性

https://caniuse.com/

#### 盒模型

总元素的宽度=width+左&右填充padding+左&右边框border+左&右边距margin

总元素的高度=height+顶部&底部填充+上&下边框+上&下边距

##### w3c盒模型

box-sizing: content-box

其实就是默认的，将padding、border、margin的值不 算入width和 height里面去。

margin0)x-sizing: content-box

标准模式下，一个块的宽度 = width+padding(内边距)+border(边框)+margin(外边距)

box-sizing: content-box

其实就是默认的，将padding、border、margin的值不 算入width和 height里面去。

##### IE的盒模型

box-sizing:border-box

就会将padding、border、margin的值算入width和 height里面去了，使得 总元素的宽度 = width的值 、总元素的高度 = height。

##### 计算尺寸

一个盒子的 margin 为 20px，border 为 1px，padding 为 10px，content 的宽为 200px、高为 50px，

如果用标准 W3C 盒子模型解释，那么这个盒子需要占据的位置为：
宽 20*2+1*2+10*2+200=262px、高 20*2+1*2*10*2+50=112px，*
*盒子的实际大小为：宽 1*2+10*2+200=222px、高 1*2+10*2+50=72px；*

如果用IE 盒子模型，那么这个盒子需要占据的位置为：
宽 20*2+200=240px、高 20*2+50=70px，
盒子的实际大小为：宽 200px、高 50px。





## flex布局

轴性布局，是一维布局

##### 两个基本概念

容器： 需要添加弹性布局的父元素；

项目： 弹性布局容器中的每一个子元素，称为项目

##### 两个基本方向

主轴： 在弹性布局中，我们会通过属性默认规定水平/垂直方向为主轴（x轴为主轴）

交叉轴： 与主轴垂直的另一方向，称为交叉轴（侧轴）（y轴为侧轴）

注意：设为 Flex布局后，子元素的float、clear和vertical-align属性将失效。但是position属性，依然生效

##### 作用于父容器的6大属性

###### flex-direction属性

决定主轴的方向（即项目的排列方向）

 row（默认值）： 主轴为水平方向，起点在左端；
 row-reverse： 主轴在水平方向，起点在右端 ；
 column：主轴为垂直方向，起点在上沿。
 column-reverse：主轴为垂直方向，起点在下沿。

###### flex-wrap属性

定义了如果一条轴线排不下，如何换行。

nowrap（默认）：不换行。当容器宽度不够时，每个项目会被挤压宽度；

wrap： 换行，并且第一行在容器最上方；

wrap-reverse： 换行，并且第一行在容器最下方。

###### flex-flow

 是flex-direction和flex-wrap的缩写形式，默认值为：flex-flow: row wrap;

###### justify-content属性

定义了项目在主轴上的对齐方式

  flex-start（默认值）： 项目位于主轴起点。

 flex-end：项目位于主轴终点。

 center： 居中

space-between：两端对齐，项目之间的间隔都相等。(开头和最后的项目，与父容器边缘没有间隔)

 space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。(开头和最后的项目，与父容器边缘有一定的间隔)

######  align-items属性

定义项目在交叉轴上如何对齐

flex-start：交叉轴的起点对齐

flex-end：交叉轴的终点对齐

center：交叉轴的中点对齐

baseline: 项目的第一行文字的基线对齐。(文字的行高、字体大小会影响每行的基线)

stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

##### 作用于项目的6大属性

###### order属性

定义项目的排列顺序。数值越小，排列越靠前，默认为0

###### flex-grow属性

定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大

###### flex-shrink属性

定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小

###### flex-basis

定义项目占据的主轴空间。(如果主轴为水平，则设置这个属性，相当于设置项目的宽度。 原width将会失效。)

######  align-self属性

定义单个项目自身在交叉轴上的排列方式，可以覆盖掉容器上的align-items属性

## grid布局

grid是二维布局

#### 容器属性



### CSS3新增伪类有那些

p:first-of-type 选择属于其父元素的首个元素
p:last-of-type 选择属于其父元素的最后元素
p:only-of-type 选择属于其父元素唯一的元素
p:only-child 选择属于其父元素的唯一子元素
p:nth-child(2) 选择属于其父元素的第二个子元素
:enabled :disabled 表单控件的禁用状态。
:checked 单选框或复选框被选中。

###  CSS3有哪些新特性？

RGBA和透明度
background-image background-origin(content-box/padding-box/border-box) background-size background-repeat
word-wrap（对长的不可分割单词换行）word-wrap：break-word
文字阴影：text-shadow： 5px 5px 5px #FF0000;（水平阴影，垂直阴影，模糊距离，阴影颜色）
font-face属性：定义自己的字体
圆角（边框半径）：border-radius 属性用于创建圆角
边框图片：border-image: url(border.png) 30 30 round
盒阴影：box-shadow: 10px 10px 5px #888888
媒体查询：定义两套css，当浏览器的尺寸变化时会采用不同的属性

transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);//旋转,缩放,定位,倾斜

### 为什么要初始化CSS样式

因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。



### rem

rem是相对长度单位
相对于根元素(即html元素)font-size计算值的倍数的一个css单位
默认浏览器以1rem=16px

### 雪碧图的优缺点

优点：
1 减少加载网页图片时对服务器的请求次数
2 提高页面的加载速度
缺点：
1 可维护性差（页面背景有少许改动，一般就要改这张合并的图片）
2 适应性差
3 小图标在高清屏幕上可能会失真，另外频繁使用定位会占用比较多的CPU

### css文件引入方式 link 和@import 

(1) link属于HTML标签，而@import是CSS提供的; 
(2) 页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;
(3) import只在IE5以上才能识别，而link是HTML标签，无兼容问题; 
(4) link方式的样式的权重 高于@import的权重.

### relative和absolute

absolute  生成绝对定位的元素， 相对于最近一级的 定位不是 static 的父元素来进行定位
fixed  生成绝对定位的元素，相对于浏览器窗口进行定位
relative  生成相对定位的元素，相对于其在普通流中的位置进行定位
static  默认值。没有定位，元素出现在正常的流中

### 优雅降级 渐进增强

优雅降级：Web站点在所有新式浏览器中都能正常工作，如果用户使用的是老式浏览器，则代码会检查以确认它们是否能正常工作。由于IE独特的盒模型布局问题，针对不同版本的IE的hack实践过优雅降级了,为那些无法支持功能的浏览器增加候选方案，使之在旧式浏览器上以某种形式降级体验却不至于完全失效.

渐进增强：从被所有浏览器支持的基本功能开始，逐步地添加那些只有新式浏览器才支持的功能,向页面增加无害于基础浏览器的额外样式和功能的。当浏览器支持时，它们会自动地呈现出来并发挥作用。

### display:none visibility:hidden

display:none  隐藏对应的元素，在文档布局中不再给它分配空间
visibility:hidden  隐藏对应的元素，但是在文档布局中仍保留原来的空间

### 关于浮动

浮动元素脱离文档流，不占据空间。

浮动元素碰到包含它的边框或者浮动元素的边框停留。

清除浮动

（1）额外标签法（会增加额外的标签使HTML结构看起来不够简洁）

```js
<div style="clear:both;"></div>
```

（2）使用after伪类

```css
#parent:after{
    content:".";
    height:0;
    visibility:hidden;
    display:block;
    clear:both;
    }
```

（3）浮动外部元素
（4）设置`overflow`为`hidden`或者auto

### 两栏布局的实现

一般两栏布局指的是**左边一栏宽度固定，右边一栏宽度自适应**

(1)利用浮动，将左边元素宽度设置为200px，并且设置向左浮动。将右边元素的margin-left设置为200px，宽度设置为auto（默认为auto，撑满整个父元素）

（2）利用浮动，左侧元素设置固定大小，并左浮动，右侧元素设置overflow: hidden; 这样右边就触发了BFC，BFC的区域不会与浮动元素发生重叠，所以两侧就不会发生重叠

（3）利用flex布局，将左边元素设置为固定宽度200px，将右边的元素设置为flex:1

（4）利用绝对定位，将父级元素设置为相对定位。左边元素设置为absolute定位，并且宽度设置为200px。将右边元素的margin-left的值设置为200px

### 三栏布局的实现

三栏布局一般指的是页面中一共有三栏，**左右两栏宽度固定，中间自适应的布局**

（1）利用**绝对定位**，左右两栏设置为绝对定位，中间设置对应方向大小的margin的值

（2）利用flex布局，左右两栏设置固定大小，中间一栏设置为flex:1

（3）利用浮动，左右两栏设置固定大小，并设置对应方向的浮动。中间一栏设置左右两个方向的margin值，注意这种方式**，中间一栏必须放到最后

### 如何解决 1px 问题



### BFC

`BFC` 是 `Block Formatting Context `的缩写，即块格式化上下文。
`BFC`是CSS布局的一个概念，是一个环境，里面的元素不会影响外面的元素。 
布局规则：Box是CSS布局的对象和基本单位，页面是由若干个Box组成的。元素的类型和display属性，决定了这个Box的类型。不同类型的Box会参与不同的`Formatting Context`。 创建：浮动元素 `display：inline-block position:absolute` 应用: 1.分属于不同的`BFC`时,可以防止`margin`重叠 2.清除内部浮动 3.自适应多栏布局

### 省略号

```css
overflow:hidden;
white-space:nowraper;
text-overflow:ellipsis;
```

### img标签在行内元素基线问题

原因是行内标签具有自身的定位，图片默认的垂直对齐方式是基线(vertical-align: baseline)

将图片转成块级元素即可(display: block)

更改图片的对齐方式为top(vertical-align: top)

### 卡片阴影

```css
{
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
 }
```

### css伪元素实现左箭头 

```css
.goBack:before {
      content: " ";
      display: inline-block;
      height: 10px;
      width: 10px;
      border-width: 2px 2px 0 0;
      border-color: #000;
      border-style: solid;
      -webkit-transform: matrix(-0.71, 0.71,0.71, 0.71, 0, 0);
      transform: matrix(-0.71, 0.71, 0.71, 0.71, 0, 0);
      position: absolute;
  }
```

### 扩大大于号小于号的角度  手机端有的不兼容

```css
{
    font-family: consolas;
  }
```

## css选择器

#### 元素选中规则

css选择器只能选中本身/后面的兄弟/后代元素

不能选中父元素，除非用js实现

:focus-within 也是一个经过子元素选择父元素的伪类，只不过条件只能是子元素是否获取焦点

```js
form:focus-within{
    background-color:black;
}
```

CSS伪类 :has() 就有这个功能，虽然还处于草案阶段，可是仍是能够提早了解一下

#### 选择器优先级

内联样式 > id选择器样式 > 类选择器样式 > 元素选择器样式

#### 相邻兄弟选择器(+)

选择紧接在另一元素后的元素，且二者有相同父元素

```css
<ul>
    <li>List item 1</li>
    <li>List item 2</li>
    <li>List item 3</li>
  </ul>
  <ol>
    <li>List item 1</li>
    <li>List item 2</li>
    <li>List item 3</li>
  </ol>
```

首先分析选择器样式：li+li{}，字面意思是选取所有位于li标签后的第一个li元素

- 很显然第一个li不会被选中，因为它前面不是li
- 第二个li会被选中，因为它是第一个li标签紧邻的li标签
- 第三个li也会被选中，因为第三个li标签的上一个标签也是li标签，也满足li+li{}的条件：li标签后的第一个li标签

注：前端面包屑导航中经常用到该选择器

#### 兄弟选择器（~），又称匹配选择器

查找某一个指定元素的后面的**所有兄弟节点**

**可以发现虽然这两个选择器都表示兄弟选择器，但是‘+’选择器则表示某元素后相邻的兄弟元素，也就是紧挨着的，是单个的（特殊情况：循环多个）。而‘~’选择器则表示某元素后所有同级的指定元素，强调所有的。**

#### 后代选择器

后代选择器是用空格分隔多个选择器组合,它的作用是在A选择器的后代中找到B选择器所指的元素

#### 子选择器(>)

只能选择作为某元素儿子元素的元素（直接子元素），不包括孙元素、曾孙元素等等等

#### 交集选择器

交集选择器是为了找两个或多个选择器的交集,用法就是把两个选择器放在一起,语法"选择器A选择器B"

```css
.list-item.active{
    color:red;
    font-size:20px
}
```

#### 并集选择器

并集选择器是为了合并类型的样式，可以多个选择器写同一种样式

```css
H1,H2,P{
    margin:0;
    padding:0;
}
```



#### 伪类选择器

##### :first-child

用于选取属于其父元素的首个子元素的指定选择器

只要E元素是它的父级的第一个子元素，就选中。它需要同时满足两个条件，一个是“**第一个子元素**”，另一个是“**这个子元素刚好是E**”

```js
 <div class="one">
    <div class="two">two</div>
    <div class="three">three</div>
  </div>
```

```css
.two:first-child{
      color: red;
    }
```

##### :nth-child(n)

该选择器选取父元素的第 N 个子元素，与类型无关

```js
<div class="one">
    <div class="two">two</div>
    <div class="two">two_2</div>
    <div class="three">three</div>
  </div>
```

```css
    .two:nth-child(1){
      color: red;
    }
```

#### 伪元素选择器

::before

::after

伪元素构造的元素是虚拟的,所以不能使用js去操作

在CSS3 中规定, 伪类用一个冒号 (:) 表示, 伪元素用两个冒号 (::)来表示

#### 属性选择器

可以为拥有指定属性的 HTML 元素设置样式

```html
 <div class="one">
    <div class="two" title="hello">two</div>
    <div class="two" data-color="blue">two_2</div>
    <div class="three">three</div>
  </div>
```

```css
 div[title='hello']{
      color: red;
    }
    div[data-color='blue']{
      color: blue;
    }
```

