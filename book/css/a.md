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
这样排列时就没有空格了
```html
<body>
    <div class="space">
        <a href=" ">a</a><a href="##">b</a><a href="##">c</a>
    </div>
</body>
```
参考：[a标签](https://www.jianshu.com/p/3df46e5b41f6)