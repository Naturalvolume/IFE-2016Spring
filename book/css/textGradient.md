# 用css实现文字渐变色
css3中新增了一些属性，可以用来实现文字渐变
### 方式一： background属性
```html
<html>
<head>
    <meta charset="utf-8">
    <style>
 
    span {
        background: linear-gradient(to right, red, blue);
        -webkit-background-clip: text;
        color: transparent;
    }
    </style>
</head>

<body>
        <span>katy is so beautiful</span>
</body>
</html>
```

##### 1. 给背景设置渐变色
`background: linear-gradient(to right, red, blue);`给背景设置渐变色，让在多个指定颜色间显示平稳过渡，以前必须使用图像来实现这些效果，通过使用css3渐变，可以减少下载时间和宽带的使用，并且渐变效果元素放大时看起来效果更好，因为渐变是由浏览器生成的，渐变的类型如下：
- 线性渐变：向下`to bottom`/向上`to top`/向左`to left`/向右`to right`/对角方向`to bottom right`/角度`angle`，通过设置起点方向/角度 和 颜色类型实现渐变效果 

`background-image: linear-gradient(direction/angle, color-stop1, color-stop2, ...);`

- 径向渐变：指定渐变中心（center）、形状（圆形或椭圆形）、大小，实现类似圆或者椭圆的渐变效果

`background-image: radial-gradient(shape size at position, start-color, ..., last-color);`

##### 2. 设置背景的绘制区域
`background-clip: border-box|padding-box|content-box;`也可以取值为`text`，表示只显示文字区域的背景，裁剪掉文字之外的区域。

##### 3. 让文字显示为透明色
`color: transparent;`让文字变为透明，显示出来背景色。

### mask
```html
<html lang="en">
<head>
<meta charset="UTF-8" />

<style type="text/css">
    h1{
        position: relative;
        color: yellow;
    }
    h1:before{
        content: attr(text);
        position: absolute;
        z-index: 10;
        color:pink;
        -webkit-mask:linear-gradient(to left, red, transparent );
    }
</style>
</head>

<body>
    <h1 text="前端简单说">前端简单说</h1>
</body>

</html>
```

`:before`选择器向选定的元素前插入内容，`content: attr(text);`指定要插入的内容，用`attr(text)`获取到`h1`元素的text属性。

css3中的`mask`属性允许用户屏蔽或裁剪特定点的图像，部分或完全隐藏某个元素的可见性，它的定义和`background`是相仿的。

设置了`mask`属性的元素上，会裁剪显示对应区域。