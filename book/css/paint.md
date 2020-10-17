# 浏览器的渲染机制和动画性能
### chrome浏览器的图层渲染机制
浏览器渲染页面时，会将页面分为很多图层，图层有大有小，每个图层上有一个或多个节点，在每个图层上做如下工作：
- 样式重计算：计算每个节点的样式
- 布局：生成每个节点的图形和位置
- 绘制：绘制填充每个节点到图层位图中
- 传给GPU：将图层作为纹理传给GPU
- 图层重组：组合图层到页面上生成最终屏幕图像

chrome中创建图层的情况：
- 3D 或 透视变换css属性
- 使用加速视频解码的`<video>`节点
- 拥有3D（webGL）上下文 或 加速的2D上下文`<canvas>`节点
- 混合插件（如flash）
- opacity
- filter

若要提高动画的性能，需要做的是减少浏览器在动画运行时所做的工作，可以用**transform**和**opacity**采用**硬件加速**。

### 两种动画的性能比较
- JS动画
  - 缺点：js在浏览器的主线程中运行，还需要运行js、样式计算、布局、绘制等，所以可能导致线程阻塞，造成丢帧。
  - 优点：可以控制动画过程：开始、暂停、回放、中止、取消，并且一些如 视差滚动效果，只有js能够完成。
- css动画
  - 缺点：缺乏对动画过程的控制能力，且难以结合，使动画变得复杂且易出问题
  - 优点：浏览器可以对动画优化，如创建图层，在主线程之外运行

参考：[css3动画优化](https://zhuanlan.zhihu.com/p/55454164)
### GPU加速的原理
GPU是一种芯片，它擅长：
- 绘制位图到屏幕
- 重复绘制同一个位图
- 在不同位置，以不同角度旋转，或不同的缩放大小来绘制同一个位图

但是GPU在**将位图加载到显存中**时速度是相对慢的。

所以，对于一些css动画属性，GPU利用自身的性能直接更改 旋转或缩放 某个图层的位图就可以，所以下面的这些属性用来做动画最好
- opacity
- translate 
- rotate
- scale

参考：[css3动画的原理](https://www.cnblogs.com/dhsz/p/6478501.html)
### 触发repaint和reflow的属性
影响reflow重布局的属性：
- 盒子模型相关的属性
  - width
  - height  
  - padding
  - margin
  - display
  - border-width
  - border  
  - min-height
- 定位相关属性和浮动
  - top
  - bottom
  - left
  - right
  - position  
  - float
  - clear
- 改变内部文字结构
  - text-align
  - overflow-y 
  - font-weight
  - overflow
  - font-family
  - line-height
  - vertical-align
  - white-space
  - font-size

这些会改变整个节点大小或位置的属性，会触发重布局。

触发重绘的属性：
- color
- border-style
- border-radius
- visibility
- text-decoration
- background
- outline
- box-shadow

这些属性对节点内部的渲染效果进行了改变，所以只需重绘。需要注意的是，**`opacity`不会触发重绘**，因为GPU只会简单降低之前已画好的纹理的`alpha`值来达到效果，并不需要整体的重绘（前提是这个被修改的`opacity`本身必须是一个图层）。

### 减少重绘回流的方法
- 样式设置
  - 避免使用层级较深的选择器，或其他复杂的选择器，提高css渲染效率
  - 避免使用css表达式，css表达式是动态设置css的属性，计算频率很快，在页面显示、缩放、滚动、移动鼠标时都要重新计算
  - 元素适当地定义高度或最小高度，否则元素的动态内容载入时，会出现页面元素的晃动或位置抖动，造成回流
  - 给图片设置尺寸，若图片不设置尺寸，首次载入时，占据空间从0到完全出现，上下左右都可能位移，发生回流
  - 不使用`table`布局，因为一个小改动可能造成整个table重新布局，且table渲染通常要3倍于同等元素时间
  - 能够使用css实现效果，尽量使用css而不使用js实现
- 渲染层
  - 将需要多次重绘的元素独立为`render layer`渲染层，如设置`absolute`，可减少重绘范围
  - 对一些进行动画的元素，使用硬件渲染，避免重绘和回流

