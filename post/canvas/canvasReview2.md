# canvas复习总结(二)

[![](https://badge.juejin.im/entry/5a7f0026f265da4e7910062f/likes.svg?style=flat)](https://juejin.im/post/5a7f0003f265da4e92682025)

## 绘制文本

* fillText(text, x, y [, maxWidth])  
在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.

* strokeText(text, x, y [, maxWidth])  
在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.


### 文本样式
* font
* textAlign
* textBaseLine
* direction

### 文本测量
measureText()  

将返回一个 TextMetrics对象的宽度、所在像素，这些体现文本特性的属性。

## 绘制图片
drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight) 

canvas可以用img，video，其他canvas获取图片源。注意，如果图片是跨域的，需要跨域图片服务器在设置允许跨域头，否则canvas画布将被污染，无法获得图片的data。

## 状态的保存和恢复
canvas使用堆栈保存状态，包括：  

* 当前应用的变形
* strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation 的值
* 当前的裁切路径

## 变形
* translate(x, y)
* rotate 顺时针方向
* scale(x, y)
* transform(m11, m12, m21, m22, dx, dy)  

	```
	m11 m21 dx
	m12 m22 dy
	0 	0 	1
	```
	m11：水平方向的缩放
	
	m12：水平方向的偏移
	
	m21：竖直方向的偏移
	
	m22：竖直方向的缩放
	
	dx：水平方向的移动
	
	dy：竖直方向的移动

* setTransform(m11, m12, m21, m22, dx, dy)  取消了当前变形,然后设置为指定的变形,一步完成。
* resetTransform() == setTransform(1, 0, 0, 1, 0, 0)


## 组合

globalCompositeOperation一共十二种模式，设定了在画新图形时采用的遮盖策略。默认值source-over

## 裁剪

clip() 用来隐藏不需要显示的部分，是canvas状态的一部分，可以被stroe保存。

## 像素操作
ImageData对象包含以下只读属性  

* width
* height
* data (Uint8ClampedArray)8位无符号整型固定数组  包含高度 × 宽度 × 4 bytes的数据

相关操作

* createImageData(width,height) 去创建一个新的，空白的ImageData对象，预设透明黑
* getImageData(left, top, width, height)
* putImageData(myImageData, dx, dy)  写入像素数据


## 抗锯齿

imageSmoothingEnabled  属性

##保存图片

* toDataURL(type, quality)
* toBlob(callback, type, encoderOptions)
