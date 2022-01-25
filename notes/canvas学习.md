# canvas

## 一，canvas介绍

1，标签：

```js
<canvas></canvas>
//注：不支持canvas的浏览器可以看的到内容，默认宽度为300px 默认高度为150px，但是在IE8下是不会显示canvas标签的，是不支持的，
//绘制环境：getContext（‘2d’）：目前支持2d的场景
//直接在标签上设置宽高是最好的，在style里面设置的话会有小问题,就是画布的大小将是等比例的大小而不是真实的大小的值。
```

## 二，canvas基础操作及实例：鼠标画线，方块移动

### 1，绘制方块

```js
fillRect(L,T,W,H):默认颜色是黑色
strokeRect(L,T,W,H):带边框的方块，默认一像素，黑色边框，显示出来的不一样的原因
```

设置绘图

```js
fillstyle:填充颜色（绘制canvas是有顺序的）
lineWidth：线宽度，是一个数值
strokeStyle:边线颜色
```

例如：

1，简单绘制

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
<style>
      body{
          background:#000;
      }      
</style>
<script>
    window.onload=function(){
    let oC=document.getElementById('cl');//查找canvas元素
    let oGC=oC.getContext('2d');//获取到canvas总方法
    //oGC.fillRect(50,50,100,100)//绘制一个黑色的正方形，距离左边50px，顶部50px，宽100px，高100px
    oGC.strokeRect(50.5,50.5,100,100);
    //绘制一个正方形边框，距离左边50.5px，顶部50.5px，宽100px，高100px
    //注：这里加.5是让边框变成一像素，如果不加的话边框就会是两像素，因为走到某个整数位置的时候边框会向两边各扩展0.5的像素，由于在canvas上画画就跟在ps上画一样，各扩展0.5的话系统会自动补全另一个对应的0.5，所以就形成了两像素
}    
</script>
</head>
<body>
  <canvas id="c1" width="400" height="400">
       <span>aaa</span>    
  </canvas>
</body>
</html>
```

2，样式修改绘制：

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
<style>
      body{
          background:#000;
      }      
</style>
<script>
    window.onload=function(){
    let oC=document.getElementById('cl');//查找canvas元素
    let oGC=oC.getContext('2d');//获取到canvas总方法
    oGC.fillStyle='red';//canvas绘制出来的矩形框是红色
    oGC.lineWidth=10;//canvas绘制出来的边框宽度是10px
    oGC.fillRect(50,50,100,100);////绘制一个红色正方形，距离左边50px，顶部50px，宽100px，高100px
    oGC.strokeRect(50.5,50.5,100,100);  
    //绘制一个正方形10px宽的边框，距离左边50.5px，顶部50.5px，宽100px，高100px
    
    //注：fillRect在前，strokeRect在后是边框覆盖内容，可以完全显示变宽，如果反过来的话就是内容覆盖边框，这样边框的宽度就不能完全显示出来
}    
</script>
</head>
<body>
  <canvas id="c1" width="400" height="400">
       <span>aaa</span>    
  </canvas>
</body>
</html>
```

### 2，边界绘制

lineJoin:边界连接点样式，miter  默认； round 圆角；bevel 斜角。

lineCap：端点样式，butt 默认；round 圆角；square 高度多出为宽一半的值

1，绘制路径1

beginPath：开始绘制路径；

closePath：结束绘制路径；

moveTo：移动到绘制的新目标点

lineTo：新的目标点

2，绘制路径2

stroke：画线，默认黑色

fill填充，默认黑色

rect：矩形区域

clearRect：删除一个画布的矩形区域

save：保存路径

restore：恢复路径

小例子1：鼠标画线

小例子2：方块移动

语法案例：

```js
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body {
      background: #000;
    }
  </style>
  <script>
    window.onload = function () {
      let oC = document.getElementById('cl'); //查找canvas元素
      let oGC = oC.getContext('2d'); //获取到canvas总方法
      oGC.fillStyle="red";//正方形颜色是红色
      oGC.strokeStyle='blue';//正方形边框是蓝色
      oGC.lineWidth=10;//正方形边框宽度是10px
      oGC.lineJoin='bevel';//正方形边框是斜角的
       oGC.fillRect(50,50,100,100);//绘制一个红色正方形，距离左边50px，顶部50px，宽100px，高100px
    oGC.strokeRect(50.5,50.5,100,100);  
    //绘制一个正方形10px宽的边框，距离左边50.5px，顶部50.5px，宽100px，高100px
  }
  </script>
</head>

<body>
  <canvas id="c1" width="400" height="400">
    <span>aaa</span>
  </canvas>
</body>

</html>
```

```js
 window.onload = function () {
      let oC = document.getElementById('cl'); //查找canvas元素
      let oGC = oC.getContext('2d'); //获取到canvas总方法
      oGC.beginPath();//开始画线
     oGC.moveTo(100,100);//先把画布点移到x为100，y为100的位置
     oGC.lineTo(200,200);//然后连线到x为200，y为200的位置
     oGC.lineTo(300,200);//然后连线到x为300，y为200的位置
     oGC.closePath();//然后再回到画布点所在的地方
     oGC.stroke()；//进行连线
  }
```

```js
 window.onload = function () {
      let oC = document.getElementById('cl'); //查找canvas元素
      let oGC = oC.getContext('2d'); //获取到canvas总方法
      oGC.lineWidth=20;//连线宽度为20px
     oGC.lineCap='square';//连线两端向外延申宽度的一半长
     oGC.moveTo(100,100);//把画布点移到x为100，y为100的位置
     oGC.lineTo(200,200);//然后连线到x为200，y为200的位置
     oGC.stroke();//进行连线
  }
```

#### 1，鼠标画线案例

```js
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body {
      background: #000;
    }
    #c1{
      background:#fff;
    }
  </style>
  <script>
    window.onload = function () {
    let oC = document.getElementById('c1'); //查找canvas元素
    let oGC = oC.getContext('2d'); //获取到canvas总方法
    oC.onmousedown=function(ev){
    var ev=ev||window.event;
    oGC.moveTo(ev.clientX-oC.offsetLeft,ev.clientY-oC.offsetTop);
    document.onmousemove=function(ev){
   var ev=ev||window.event;
    oGC.lineTo(ev.clientX-oC.offsetLeft,ev.clientY-oC.offsetTop);
    oGC.stroke();
    }
    document.onmouseup=function(){
    document.onmousemove=null;
    document,onmouseup=null;
    }
    }
    }
  </script>
</head>

<body>
  <canvas id="c1" width="400" height="400">
    <span>aaa</span>
  </canvas>
</body>

</html>
```

#### 2，方块移动

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body{
      background: #000;
    }
    .cl{
      background: #fff;
    }
  </style>
</head>
<body>
  <canvas class="cl" width="400" height="500"></canvas>
</body>
</html>
<script>
  let cl=document.querySelector('.cl');//查找画布元素
  let clPaint=cl.getContext('2d');//调用画布方法
  let num=0;
  let time=setInterval(() => {
    num++;
    clPaint.clearRect(0,0,cl.width,cl.height);//在方块每走一步都清楚掉原来的方块
    clPaint.fillRect(num,num,100,100);//绘制方块
    if(num==300){
      clearInterval(time);
    }
  }, 30);
</script>
```

## 三，canvas绘制圆形及实例：canvas时钟、其他曲线、变换、缩放

### 1,绘制圆

arc(x,y,半径，起始弧度，结束弧度，旋转方向)

​    ---x,y:起始位置

   ---弧度与角度的关系：弧度=角度*Math.PI/180

   ---旋转方向：顺时针（默认：false）、逆时针（true）

  ---例子：用arc去画个钟表

#### 钟表案例

```js
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body {
      background: #000;
    }

    .cl {
      display: block;
      background: #fff;
      margin: 0 auto;
    }
  </style>
</head>

<body>
  <canvas class="cl" width="500" height="500"></canvas>
</body>

</html>
<script>
  let cl = document.querySelector('.cl');
  let clPaint = cl.getContext('2d');

  function toDraw() {
    let x = 200; //绘制圆心起始位置
    let y = 200;
    let r = 150; //绘制半径

    clPaint.clearRect(0,0,cl.width,cl.height);//每走一次就清楚之前走过的画布

    let oDate = new Date(); //获取时间对象
    let oHours = oDate.getHours(); //获取小时
    let oMin = oDate.getMinutes(); //获取分钟
    let oSencond = oDate.getSeconds(); //获取秒钟

    let oHoursValue = (-90 + oHours * 30+oMin/2) * Math.PI / 180; //获取小时弧度
    let oMinValue = (-90 + oMin * 6) * Math.PI / 180; //获取分钟弧度
    let oSenValue = (-90 + oSencond * 6) * Math.PI / 180; //获取秒钟弧度

                          /////////////////////////////小刻度制作
    clPaint.lineWidth = "1"
    clPaint.beginPath(); //画出每6度一个秒刻度
    for (let i = 0; i < 60; i++) {
      clPaint.moveTo(x, y);
      clPaint.arc(x, y, r, 6 * i * Math.PI / 180, 6 * (i + 1) * Math.PI / 180, false);

    };
    clPaint.closePath();
    clPaint.stroke();

    clPaint.fillStyle = "white"; //填充色为白色
    clPaint.beginPath(); //画填充秒刻度的填充圆
    clPaint.moveTo(x, y);
    clPaint.arc(x, y, r * 19 / 20, 0, 360 * Math.PI / 180, false)
    clPaint.closePath();
    clPaint.fill();
    
                           /////////////////////////////大刻度制作
    clPaint.lineWidth = "3"
    clPaint.beginPath(); //画出每30度一个时刻度
    for (let i = 0; i < 12; i++) {
      clPaint.moveTo(x, y);
      clPaint.arc(x, y, r, 30 * i * Math.PI / 180, 30 * (i + 1) *
        Math.PI / 180, false);
    };
    clPaint.closePath();
    clPaint.stroke();

    clPaint.fillStyle = "white"; //填充色为白色
    clPaint.beginPath(); //画填充时刻度的填充圆
    clPaint.moveTo(x, y);
    clPaint.arc(x, y, r * 18 / 20, 0, 360 * Math.PI / 180, false)
    clPaint.closePath();
    clPaint.fill();

    /////////////////////////////秒针制作
    clPaint.lineWidth = "1";
    clPaint.beginPath();
    clPaint.moveTo(x, y);
    clPaint.arc(x, y, r * 18 / 20, oSenValue, oSenValue, false)
    clPaint.closePath();
    clPaint.stroke();

    /////////////////////////////分针制作
    clPaint.lineWidth = "2";
    clPaint.beginPath();
    clPaint.moveTo(x, y);
    clPaint.arc(x, y, r * 14 / 20, oMinValue, oMinValue, false)
    clPaint.closePath();
    clPaint.stroke();

    /////////////////////////////时针制作
    clPaint.lineWidth = "3";
    clPaint.beginPath();
    clPaint.moveTo(x, y);
    clPaint.arc(x, y, r * 10 / 20, oHoursValue, oHoursValue, false)
    clPaint.closePath();
    clPaint.stroke();
  }
  setInterval(toDraw, 1000);
  toDraw();
</script>
```

### 2，绘制其他曲线

arcTo（x1,y1,x2,y2,r）

   --第一组坐标、第二组坐标、半径

quadraticCurveTo（dx,dy,x1,y1）

  --贝塞尔曲线：第一组控制点、第二点结束坐标

bezierCurveTo(dx1,dy1,dx2,dy2,x1,y1)

 --贝塞尔曲线：第一组控制点、第二组控制点、第三组结束坐标

### 3，旋转变换

translate

​      ------偏移：从起始点为基准点，移动当前坐标位置

rotate

   ------旋转：参数为弧度

  ------ 例子：旋转的小方块

scale

   ------缩放例子：旋转加缩放的小方块

#### 方块移动旋转案例

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>rect-move</title>
  <style>
    body{
      background:#000;
    }
    .cl{
      /* display: block; */
      background:#fff;
      /* margin:0 auto; */
    }
  </style>
</head>
<body>
  <canvas class="cl" height="500" width="500"></canvas>
</body>
</html>
<script>
  var cl=document.querySelector('.cl');
  var clPaint=cl.getContext('2d');
  var num=0;//定义起始度数为0度
  var num2=0;
  var value=1;

  clPaint.translate(100,100);//第一次把转点移到100，100的位置

   setInterval(function(){
     num++;//角度累加
     clPaint.save();//用save和restore去除方块原来旋转的角度
     clPaint.clearRect(0,0,cl.width,cl.height);
    //  if(num2==100){
    //    value=-1;
    //  }else if(num2==0){
    //    value=1;
    //  }
    //  num2+=value;
    //  clPaint.scale(num2*1/50,num2*1/50);
     clPaint.rotate(num*Math.PI/180);//旋转角度依次累加
    //  clPaint.translate(-50,-50);//然后每转一次就减少50
     clPaint.fillRect(0,0,100,100);//旋转后在画方块
       clPint.restore();
   },30);
</script>
```

## 四，canvas图片及实例：图片360度旋转、设置背景、渐变、文本、阴影

### 1，插入图片

等图片加载完成，再执行canvas操作

   ------图片预加载：在onload中调用方法

drawlmage（olmg，x，y，w，h）

​    ---olmg：当前图片；        x，y：坐标；         w，h：宽高

   -----例子：微博的图片旋转效果

### 2，设置背景

 createPattern（olmg，平铺方式）

​    ---2参为：repeat、repeat-x、repeat-y、no-repeat

## 五、如何通过LMT进行信令跟踪

### 1，应用场景

介绍如何通过LMT进行信令跟踪。信令跟踪是故障定位的一种重要手段。通过具体的信令进行跟踪可分析可能出现问题的环节。

### 2，配置思路

a,SIP信令跟踪

b，QSIG信令跟踪

c，BRI信令跟踪

d，PRA信令跟踪

e，SS7信令跟踪

f，R2信令跟踪

### 3，选择类型

1，选择类型为Link，通过show mtplink可查看对应的Index值；

2，选择类型为DPC，通过show dsp查看对应的Index值。

注：选择类型为DPC，则表示通过指定目的信令点进行信令收集，此时收集到的信令都是从指定索引的信令点发出的信令或发向该信令点的信令。

```js
起始/终止时间：需要查找或过滤某个时间段内的信令信息时，设置的开始/终止时间点。
方向：对于当前工具连接设备而言，是发送信令还是接收信令。
CIC：信令的电路识别码。同一条中继电路的CIC编码必须完全一致才能成功对接。
取值范围：0~4095之间的整数，起始CIC默认值：0.
信令类型：SS7信令的类型，如ACM,IAM,CPG。
主叫/被叫号码：呼叫发起方/接收方的号码。
```



