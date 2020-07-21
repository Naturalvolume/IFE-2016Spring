## a 标签周围为什么会有空格
首先，a 标签是行内元素，而行内元素间的回车键、空格、制表符等在渲染时会按照一个空格处理，所以当按下面的格式排列时，a 标签间就会出现一个空格的间隙。
```html
<body>
    <div class="space">
        <a href=" ">a</a>
        <a href="##">b</a>
        <a href="##">c</a>
    </div>
</body>
```
所以，两个**display：inline-block**元素放到一起也会产生一段空白。
### 解决方法
1. 两个元素紧挨着写就没有空白了
```html
<body>
    <div class="space">
        <a href=" ">a</a><a href="##">b</a><a href="##">c</a>
    </div>
</body>
```
参考：[a标签](https://www.jianshu.com/p/3df46e5b41f6)
2. 父元素中设置font-size: 0，在子元素上重置正确的font-size
3. 为子元素设置float:left
```css
.left{
  float: left;
  font-size: 14px;
  background: red;
  display: inline-block;
  width: 100px;
  height: 100px;
}
```