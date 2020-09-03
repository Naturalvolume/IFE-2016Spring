# 两栏等高布局
在两栏布局中，实现以最高一栏的高度为准，两栏的背景色与容器高度相等。不能用`height: 100%`，因为必须给父元素设定具体高度值。
### 用 margin-bottom 和 padding-bottom 实现
- 左右元素左浮动实现两栏布局
- 等高实现
  - padding值可以扩大元素背景颜色的显示范围，所以给两个元素加上大的padding-bottom保证最大范围内两者同高度
  - 再加上一个绝对值相等的相反数给 margin-bottom，实现内容在原来的位置上
- 父元素要设置 overflow:hidden:
  - 触发父元素的BFC，使高度自适应浮动元素高度
  - 将超过部分隐藏，主要是隐藏多余的背景色
```html
<div class="container">
  <div class="son left"></div>
  <div class="son right"></div>
</div>
```
```css
.container {
  overflow: hidden;
}
.son {
  margin-bottom: -9999px;
  padding-bottom: 9999px;

  width: 50%;
  float: left;
}
.left {
  background-color: pink;
}
.right {
  background-color: lightblue;
}
```