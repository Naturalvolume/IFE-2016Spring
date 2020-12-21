# 图片懒加载
图片懒加载是一种性能优化的形式，当加载大量图片时很有用，这里用与元素尺寸相关的属性实现。
### 网页中元素的尺寸
每个html元素都具有clientHeight、offsetHeight、scrollHeight、offsetTop、scrollTop这5个元素高度、滚动、位置相关的属性。
- element.clientHeight: 表示element元素包括padding但不包括border、水平滚动条、margin的元素的高度（和元素滚动、位置没有关系）    ——常用来获取某个**父元素内容的宽高**
- element.offsetHeight: 表示element元素包括padding、border、水平滚动条，但不包括margin的元素的高度（和元素滚动、位置没有关系）    ——常用来获取**子元素整体宽高**
- element.offsetTop: 当前元素顶部距父元素顶部的距离（和有无滚动条无关，但会随着滚动条改变）
当元素超过父元素的高度，父元素就会出现滚动条，在滚动过程中元素有部分被隐藏，以下几个属性才有意义
- element.scrollHeight: 包括当前不可见部分的元素的高度，而可见部分高度就是clientHeight，也就是scrollHeight >= clientHeight是一直成立的（没有滚动条时 scrollHeight == clientHeight）
- element.scrollTop: 有滚动条时，滚动条向下滚动的距离也就是元素顶部被遮住部分的高度，也即被滚动条滚走的距离（没有滚动条时 scrollTop == 0恒成立）

### 实现
- 给图片一个占位资源
```css
/* data-src就是自定义属性，用来存放真正需要显示的图片路径 */
<img src="default.jpg" data-src="http://www.xxx.com/target.jpg" /></img>
```
- 通过scroll事件判断图片是否到达视口
```javascript
// 获取所有img元素
let img = document.document.getElementsByTagName("img");
let count = 0;//计数器，从第一张图片开始计

lazyload();//首次加载别忘了显示图片

window.addEventListener('scroll', lazyload);

function lazyload() {
  let viewHeight = document.documentElement.clientHeight;//视口高度
  // Doctype设置不同，对从哪里获得滚动条scrollTop有影响
  let scrollTop = document.documentElement.scrollTop || document.body.scrollTop;//滚动条卷去的高度
  for(let i = count; i <num; i++) {
    // 元素现在已经出现在视口中
    if(img[i].offsetTop < scrollHeight + viewHeight) {
      // 图片已加载过
      if(img[i].getAttribute("src") !== "default.jpg") continue;
      // 从 data-src 属性获取图片的地址，加载图片
      img[i].src = img[i].getAttribute("data-src")
      // 删除原来图片属性
      img[i].addEventListener("load", function() {
        img[i].removeAttribute("data-src")
      })
      // 更新索引
      count++;
    }
  }
}
```

- 加上节流，避免频繁触发

为什么要用节流，而不是防抖
```javascript
window.addEventListener('scroll', throttle(lazyload, 200));
```
### documentElement和body的异同点
- document是整个文档（网页的整个网页结构）
- document.documentElement代表整个文档树的根节点，在网页中即html标签
- document.body表示dom节点树的body节点

### 当懒加载尺寸未知的图片时
加载已知尺寸图片时，可以加载对应尺寸的placeholder图片，通过自己裁剪对应尺寸的placeholder图片。

在图片尺寸未知时，浏览器难以计算需要给图片预留的位置，所以当图片加载完成后会出现网页布局的抖动。
为了避免布局闪动，可采用aspect ratio boxes制作一个占位用元素。

```html
<div class="lazy-load__container feature">
  <img src="placeholder.jpg" src="real.jpg" />
</div>
```

```css
.lazy-load__container{
    position: relative;
    display: block;
    height: 0;
}

.lazy-load__container.feature {
    // feature image 的高宽比设置成42.8%
    // 对于其他图片 比如 post图片，高宽比可能会不同，可以使用其他css class去设置
    padding-bottom: 42.8%;
}

.lazy-load__container img {
    position: absolute;
    top:0;
    left:0;
    height: 100%;
    width: 100%;
```

实现原理：`padding-bottom`的百分比是根据元素生成`box`的`width`去计算的，所以这个container的具体尺寸会由尺寸确定的外层元素确定，但是宽高比始终保持一致。

优点：图片加载时防止高度塌陷，避免布局的抖动
缺点：不能适配图片比例不一致的网站。

[图片懒加载从简单到复杂](https://mp.weixin.qq.com/s/FD9YYHPuUzBGBdSUalzTSg)
### Medium懒加载图片
Medium是国外的一个英文创作网站，类似于知乎，它的懒加载效果非常好，主要是通过几种技术组合实现。
- 使用aspect ratio box 创建占位元素
- html解析时只加载一个小尺寸的图片（只有原图20%大小的缩略图），在img标签中直接被加载，所以网页一打开便显示该图片
- 图片加载后，在`<canvas>`中绘制图像，然后通过自定义`blur`函数获取图像数据并加载，并且添加blur效果
- 加载主图像后，显示该图像并隐藏画布，应用css3动画，所以整体效果看起来非常流畅

[Medium是如何进行渐进式图像加载的](https://baijiahao.baidu.com/s?id=1608281115769138285&wfr=spider&for=pc)