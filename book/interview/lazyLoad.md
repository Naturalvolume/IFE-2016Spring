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
      img[i].src = img[i].getAttribute("data-src");
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
