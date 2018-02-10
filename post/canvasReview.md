# canvas复习总结(一)

最近一段时间一直在做echarts绘图方面的东西，对canvas非常感兴趣，但是原生的接口也忘得差不多了，在此总结整理下。

###width and height
canvas的width和height属性是画布实际宽度和高度，我们在这个上面绘制的图形。如果没有设置，默认为width:300px,height:150px。而canvas的style的width和height是canvas在浏览器中被渲染的高度和宽度。
###高分屏下变模糊的处理
canvas是位图，所以无法缩放保持不变形。在如mac上，浏览器就会以2个像素点的宽度来渲染一个像素，canvas在Retina屏幕下相当于占据了2倍的空间，因此变得模糊。因此，要做Retina屏适配，关键是知道当前canvas的实际渲染倍率，然后将canvas放大到该倍率来绘制，最后将canvas压缩成一倍的物理大小来展示。canvas中的线条大小、文字大小等都需要乘以该倍率来进行绘制。  

* 实际渲染倍率的计算
```js
ratio = window.devicePixelRatio / (context.backingStoreRatio || 1);
```  

* 缩放canvas  

```js
canvas.style.width = canvas.width + 'px';  

canvas.style.height = canvas.height + 'px';

canvas.width = canvas.width * ratio;  

canvas.height = canvas.height * ratio;
```
注意style上一定要加单位，不然你会看到惊喜。
###渲染上下文
```js
getContext('2d')
getContext('webgl')
//目前浏览器未实现
getContext('webgl2')
```
###栅格 and 坐标空间
栅格的起点为左上角（坐标为（0,0）)
![坐标空间](https://github.com/lgl1993/blog/blob/master/assets/Canvas_default_grid.png)

###矩形
x与y指定了在canvas画布上所绘制的矩形的左上角（相对于原点）的坐标。width和height设置矩形的尺寸。  
 
* fillRect
* strokeRect
* clearRect
* rect   (属于路径范畴)

```
fillRect(x, y, width, height)
绘制一个填充的矩形

strokeRect(x, y, width, height)
绘制一个矩形的边框

clearRect(x, y, width, height)
清除指定矩形区域，让清除部分完全透明

rect(x, y, width, height)
将一个矩形路径增加到当前路径上，执行后moveTo()方法自动设置坐标参数为（0,0）
```
rect与其他不同的是rect是路径，二其他的都可以直接在canvas上绘制出来。

###路径
* beginPath
* closePath  

```
beginPath()
新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
closePath()
闭合路径之后图形绘制命令又重新指向到上下文中。
这个方法会通过绘制一条从当前点到开始点的直线来闭合图形。
```
当我们在调用fill时会自动闭合路径，stroke不会闭合。  

![图片1](https://github.com/lgl1993/blog/blob/master/assets/canvas1.png)
![图片2](https://github.com/lgl1993/blog/blob/master/assets/canvas2.png)  

第二张图片中，绘制圆形前没有使用beginpath的函数，因此stroke时会连起来。
####移动笔触
* moveTo(x,y)  移动当前画笔位置

####线
* lineTo(x,y)

####圆弧
* arc(x, y, radius, startAngle, endAngle, anticlockwise)  


画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。Angle单位是弧度。

####二次贝塞尔曲线及三次贝塞尔曲线
```js
quadraticCurveTo(cp1x, cp1y, x, y)
绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点。
bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。
```
![图片描述](https://github.com/lgl1993/blog/blob/master/assets/Canvas_curves.png)
红色点为控制点，蓝色为起始点和终点。

