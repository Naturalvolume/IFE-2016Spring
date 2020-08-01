# canvas
canvas元素自身属性和一般的dom元素没有很大区别，绘图api并不再canvas元素自身定义，而是定义在一个 CanvasRenderingContext2D 对象上（上下文对象），该对象通过getContext()方法获得。
### 画板坐标系
有当前坐标系和默认坐标系，默认坐标系就在左上角，当前坐标系指图像绘制时候的参考坐标系，作为图像状态的一部分，比如 rotate 旋转操作就是相对于参考坐标系的。
- translate(x, y) 将当前坐标系原点上下左右移动
- setTransform(a, b, c, d, e, f) 通过放射变换原理，移动默认坐标系原点。
```
x' = ax + cy +e
y' = bx + dy +f
```
### 设置画板宽高
画板宽高要在行内设置，不能在style中设置。查了一些资料，我个人是这样理解的，`canvas`默认宽高为`300*150`（相当于定义在行内），在style中定义的宽高，是设置到了`canvas`的上一层上，如果设置的宽高比例不是默认的比例，就会造成扭曲，最后画出来的图形坐标也是不对的。
### 保存图像属性
通过 getContext() 方法只能获得一个上下文对象，画笔、粗细等都是这个对象的属性，所以上下文同一时刻只能有一个属性即画笔。所以想要使用其他图像属性（另一支画笔）时，只能通过保存当前图像状态，然后新建一个图像状态切换。

借助 save() 和 restore() 切换图像状态，每次save()都保存当前图像状态（博阿酷哦图像属性、当前坐标系、裁剪区域等信息）；restore() 切换到最近图像状态。
### 画图
1. 双层画板
2. 铅笔等不同模式的选择
### 清除画板
用`clearRect(0, 0, this.widthSize, this.heightSize)`矩形清除到方式清除
### 下载画板
用一个**不显示**的超链接，把它的地址设置成`href='javascript:void(0);`，阻止误触时的默认跳转动作，当点击下载按钮时，再把超链接当地址设置成`this.$refs.download.href = this.canvas.toDataURL()`画板的编码后的url地址，主动触发超链接的点击事件`this.$refs.download.click()`，发现不是可以跳转的格式，就会自动下载画板。
### 像素操作
ImageData对象包含了一个区域内的canvas像素信息，通过下面两种方式创建该对象
```javascript
// 1.获取整个画板的像素信息
var myImageData = ctx.createImageData(width, height);
// 2.截取left,top区域的像素信息
var myImageData = ctx.getImageData(left, top, width, height);
// 3.复制另一个画板的宽度和高度，不能复制data属性，所以 像素信息全是透明黑色
var myImageData = ctx.createImageData(anotherImageData);
```
该对象中有一个data属性，以数组形式存储所有像素的rgba数值，data[0]是第一排第一列像素r通道的数值，data[1]是第一排第一列像素g通道的数值，data[2]是第一排第一列像素b通道数值，data[3]是alpha通道。根据对象的width和height属性可以推断出其中每一个像素的值。

参考：[ImageData对象](https://www.cnblogs.com/-867259206/p/7448270.html)
