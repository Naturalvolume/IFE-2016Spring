# 移动端开发要点
### css3新特性
- 新选择器：[新增属性选择器、结构伪类选择器](https://www.cnblogs.com/liutianzeng/p/10933351.html)
- 边框、背景、圆角、阴影
- 新的盒模型：flex
- 渐变、动画、2D3D转换
- 新单位：rem(根元素html的font-size)、vw(视口宽度)、vh(视口高度)
- 在线字体图标：如阿里的iconfont，生成在线链接后引入
- 前缀应用、浏览器兼容、渐进增强、优雅降级（移动端发展快，所以兼容性好）

渐进增强：先针对低版本浏览器构建页面，完成基本功能，再针对高级浏览器进行效果、交互、追加功能达到更好的体验。
优雅降级：一开始就使用css3构建完整功能，然后针对浏览器进行hack让其可以在低版本浏览器上正常浏览。
```css
/* 在浏览器发展历史中，是先支持前缀css3，后来才支持正常css3的 */
.transition { /*渐进增强写法*/
      -webkit-transition: all .5s;
      -moz-transition: all .5s;
      -o-transition: all .5s;
         transition: all .5s;
}
.transition { /*优雅降级写法*/
          transition: all .5s;
       -o-transition: all .5s;
     -moz-transition: all .5s;
  -webkit-transition: all .5s;
```

- 媒体查询：根据用户设备的类别(screen、print、TV、盲人用、投影设备)和尺寸调用不同的样式，**是自适应的基础**，最常用的查询是处理视口高度和宽度。

```css
@media screen and (min-width: 768px)  {
  /* 当屏幕宽度小于768时，改变背景颜色 */
  body {
    background-color:lightblue;
  }
}

@media screen and (max-aspect-ratio: 1200/1000) {
        //宽高比常规用法
}
```
### 移动端适配开发方案
PC端的视口指的是可视区域，宽度和浏览器窗口的宽度保持一致。
而移动端涉及到三个视口：
- 布局视口：为了解决早期页面在手机上显示的问题，默认设置viewport标签，PC端、iOS、Android 基本都将这个视口分辨率设置为 980px，所以 PC 上的网页基本能在手机上呈现，只不过元素看上去很小，一般默认可以通过手动缩放网页,通过`document.documentElement.clientWidth`获取
- 可视视口：取决于屏幕大小，通过window.innerWidth获取
- 理想视口：不同设备拥有不同的理想视口，是最适合移动设备的视口，取决于移动设备的屏幕宽度、高度（个人理解，缩放为1时可视视口和理想视口是一样的，所以理想视口只是提供一个缩放功能）

两个像素：
- 物理像素：指设备屏幕的物理像素
- css像素：和物理像素之间的比例取决于屏幕的特性以及用户是否进行缩放，由浏览器自行换算

两个比例
- 设备逻辑像素（device independent pixel, dip） ＝ 一个物理像素 ／ 一个css像素
- 缩放值 ＝ 理想视口宽度 ／ 可视视口宽度

通过viewport设置成理想视口
```css
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">

/* 一样的效果 */
<meta name="viewport" content="width=device-width">
```

[移动前端开发之viewport的深入理解](https://www.cnblogs.com/2050/p/3877280.html)
[浅谈移动端中的视口（viewport）](https://www.cnblogs.com/yuduxyz/p/9745962.html)
### 响应式布局开发方案（PC端＋移动端）
将三种已有的开发技术整合起来，弹性布局、弹性图片、媒体和媒体查询，覆盖了自适应，可以根据屏幕的大小自动调整页面的展示方式以及布局（**响应式解决了自适应布局的问题**，可以自动识别屏幕宽度，做出相应调整，布局和展示的内容可能会有变动）。

响应式布局步骤：
- 设置meta标签，移动端适配
- 通过媒体查询根据设备和宽度等条件设置样式(设置多种视图宽度)
- 使用flex（兼容性差）
- 使用百分比布局或rem布局

响应式布局方案：
- 媒体查询：根据不同宽度实现不同样式（变化不够流畅）
- 百分比布局：子元素随父元素尺寸变化（计算困难）
- rem布局：根元素的font-size提供基准，页面size变化时，改变font-size的值，以rem为固定单位的元素大小发生响应式变化（须通过js动态控制根元素的大小，css和js耦合）
- 视口单位：css3中引入的新单位vw/wh，vw是相对视口的宽度（1vw等于视口宽度的1%）

[自适应和响应式布局](https://www.cnblogs.com/guisenbin/p/10451731.html)
[响应式布局](https://www.jianshu.com/p/daf1119e187b?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)
### 移动端js、触屏事件
1.click事件
点击事件类似于pc端，但在移动端，连续click的触发有200ms-300ms的延迟（确认是否为双击事件）

2.touch事件
触屏事件touch是一组事件
- touchstart: 触摸开始（手指放在触摸屏上）
- touchmove: 拖动（手指在触摸屏上移动）
- touchend: 触摸结束（手指从触摸屏上移开）
- touchenter ：移动的手指进入一个dom元素
- touchleave ：移动的手指离开一个dom元素
- touchcancel: 拖动中断时候触发（如忽然有alert）

touch事件对象
- TouchList：触摸点（一个手指触摸就是一个触发点，和屏幕的接触点的个数）的集合
- changedTouches：改变后的触摸点集合
- targetTouches：当前元素的触发点集合
- touches: 页面上所有触发点集合

3.tap事件
- tap：手指长按屏幕触发
- longTap: 手指长按屏幕会触发
- singleTap: 手指碰一下屏幕会触发
- doubleTap: 手指双击屏幕会触发

4.swipe类事件
滑动事件
- swipe：手指在屏幕上滑动时会触发
- swipeLeft：手指在屏幕上向左滑动时会触发
- swipeRight：向右滑动触发
- swipeUp：向上滑动触发
- swipeDown：向下滑动

##### 事件触发顺序
单击：touchstart -> touchend -> click -> tap -> singleTap
双击：touchstart -> touchend -> click(延迟，只触发一次) -> tap -> touchstart -> touchend -> tap -> doubleTap
长按：touchstart -> longTap -> touchend
右滑：touchstart -> touchmove -> touchend -> swipe -> swipeRight
长按无意触发浏览器自身复制文本功能：touchstart -> touchcancel
### 移动端库
#### vue中配置视口：
- 安装PostCSS插件
- 在.postcssrc.js文件对新安装的PostCSS插件进行配置
[如何在Vue项目中使用vw实现移动端适配](https://www.cnblogs.com/yikuu/p/9052148.html)
#### swiper
成熟稳定的应用于PC端和移动端的滑动效果插件，一般用来触屏焦点图、触屏整屏滚动等效果。可应用于移动网站、webapp、native app或者hybrid app。


### 移动端开发调试