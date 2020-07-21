# 三栏布局
三栏布局一般有以下几个要求：
- 两栏宽度固定，最好中间栏能自适应布局
- 实现效果前提下，尽可能让中间栏优先渲染（一般中间栏是页面主页）
- 三栏的高度一般是由具体内容决定的，所以三栏的高度常常会不统一（在下面的程序中确定高度是为了方便显示）
- 只需要一个父元素 div 包裹

### 1.中间和右边绝对定位
```html
<div class="container">
  <div class="center"></div>
  <div class="left-part"></div>
  <div class="right-part"></div>
</div>
```
```css
    .container {
      /* 父元素相对定位 */
      position: relative;
    }
    .center {
      /* 中间绝对定位 */ 
      position: absolute;
      /* 具体高度根据内容决定 */ 
      height: 300px;
      /* 相对父元素的，left、right 是左右盒的宽度 */ 
      left:200px;
      right:200px;
      background: red;
    }
    /* 左侧根据 常规流 正常定位 */ 
    .left-part{
      width: 200px;
      height:300px;
      background: yellow;
    }
    /* 右侧绝对定位 */ 
    .right-part {
      width: 200px;
      height: 300px;
      background: blue;
      position:absolute;
      /* 一定要确定 top 和 right */ 
      top: 0;
      right:0;
    }
```
**优点**：中间盒子先渲染
**缺点**：若高度不固定，三栏的高度会随内容变得不一致，不美观
**问题**：中间栏和右侧栏都是固定定位，为什么中间栏不需要定义 top:0，而右侧栏要定义 top:0 才能正常显示呢？（可以删掉top试一下）
### 2.两侧浮动
```html
<div class="container">
  <div class="left-part"></div>
  <div class="right-part"></div>
  <div class="center"></div>
</div>
```
```css
    .container {
    }
    .center {
      height: 300px;
      background-color: red;
    }
    .left-part {
      float: left;
      width: 200px;
      height: 300px;
      background-color: yellow;
    }
    .right-part {
      float: right;
      width: 200px;
      height: 300px;
      background-color: blue;
    }
```
**缺点**：中间盒子最后才渲染，且三栏高度仍不统一。
**问题**：如果把中间盒子的 html 元素放到最上方，会发生什么呢？为什么会这样？这样有什么缺点？
### 3.圣杯布局
圣杯布局是非常典型的一个三栏布局，中间有非常多的技巧。
```html
<div id="header"></div>
<div id="container">
  <div id="center" class="column"></div>
  <div id="left" class="column"></div>
  <div id="right" class="column"></div>
</div>
<div id="footer"></div>
```
```css
body {
  min-width: 550px;
}

#container {
  /* 用父元素的内边距给左右栏预留位置 */
  padding-left: 200px; 
  padding-right: 150px;
}

/* 三栏全部左浮动 */
#container .column {
  float: left;
  /* 方便显示 */
  height: 300px;
}

#center {
  /* 中间栏用百分比自适应显示 */
  width: 100%;
  background: red;
}

#left {
  width: 200px; 
  margin-left: 100%;
  /* 一直以为浮动元素不能用定位了呢，孤陋寡闻了 */
  position: relative;
  right: 200px;
  background: yellow;
}

#right {
  width: 150px; 
  margin-left: -150px; 
  background: blue;
  position: relative;
  left: 150px;
}

#footer {
  clear: both;
}
```
**优点**：中间盒子先渲染
**缺点**：若高度不固定，三栏的高度会随内容变得不一致，不美观

**浮动 float 的原理**：
float，浮动，使元素的边界和相邻的同一个方向浮动的元素边界紧贴，如果没有相邻浮动元素，就和父元素边界紧贴。也就是说，设置了 float：left 的元素，会向左浮动，直到左边缘和同样设置了 float：left 的元素紧贴。如果左边没有浮动的元素，那么这个元素就会紧贴到父元素的边缘 
**margin-left: -100%的原理**：
这里 100% 的百分比是相对于父元素的宽度而言的。元素设置了 margin-left: -100%，就会往左边移动父元素的宽度的100%

**问题**：
- 为什么要给 body 设置 min-width: 550px;
- 中间栏设置宽度 width: 100%; 是什么意思？若设置 width:1200px 会有什么现象？
- 为什么 footer 要设置 clear:both，还有哪些方法可以实现同样的效果？

### 4.双飞翼布局
双飞翼布局和圣杯布局非常相似，两者最大的区别是用 margin 还是用 padding 给左右栏预留位置，以及只需要用负边距固定两侧栏，不需要用定位。
```html
  <div id="header"></div>
  <div id="container" class="column">
    <div id="center"></div>
  </div>
  <div id="left" class="column"></div>
  <div id="right" class="column"></div>
  <div id="footer"></div>
```
```css
body {
  min-width: 500px;
}
/* 中间栏用父元素包裹 */
#container {
  width: 100%;
}
/* 中间栏相对父元素左右边距 */        
#center {
  margin-left: 200px;
  margin-right: 150px;
}
/* 所有元素左浮动 */
.column {
  float: left;
}
       
#left {
  width: 200px; 
  margin-left: -100%;
}
        
#right {
  width: 150px; 
  margin-left: -150px;
}
        
#footer {
  clear: both;
}
```
**问题**：圣杯布局 和 双飞翼布局的区别到底在哪里呢？它们各有什么优势？
参考：[圣杯布局和双飞翼布局的理解与思考](https://www.jianshu.com/p/81ef7e7094e8)

### 5. flex 布局
```html
<div class="container">
  <div class="center column"></div>
  <div class="left column"></div>  
  <div class="right column"></div>
</div>
```
```css
  .container {
    display: flex;
    justify-content: space-between;
  }
  .column {
    height: 300px;
  }
  .left {
    order: 1;
    background: yellow;
    width:200px;
  }
  .center {
    order: 2;
    background: red;
    flex: 1;
  }
  .right {
    order: 3;
    background: blue;
    width: 200px;
  }
```
**问题**：
- html 中中间栏center是第一个dom元素，按照我们所知道常规流布局规则，中间栏应该在左边，为什么最后渲染出来的中间栏却是在中间？
- 在 center 中定义 `flex:1`是什么意思？如果去掉它会有什么效果？

### 总结
三栏布局还有 grid布局 table-cell布局的实现方式，感兴趣了可以看看，上面列的三栏布局实现方式，都有一个共同的缺点，就是无法统一三栏的高度，那么有没有什么方法能够实现统一三栏的高度呢？另外，你能想到哪些两栏布局的方法呢？