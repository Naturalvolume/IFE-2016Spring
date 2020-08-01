# 图标编码问题
<!-- unicode直接在标签中使用，可以识别这是unicode编码
            在对象中使用再引入，因为js只能处理UCS-2编码，所以在遍历过程中不能识别unicode编码
            索引在使用前，要先转码
          -->

在标签中可以直接使用unicode字符，如`<span class="iconfont">&#xe7cd;</span>`，但是在`v-for`循环中用对象取unicode编码时就不能在页面中显示出正确的图标了，这是因为：只能在标签中才能使用unicode，而javascript只能处理UCS-2编码。
```
一个unicode为e70b
那么它在标签中的输入方式为:&#xe70b;（unicode的前缀 &#x）

let obj = {icon: '&#xe70b;'}
假如在对象中用unicode，那么就在标签中使用{{String.fromCharCode(parseInt(e70b,16))}}这种写法让其转码（在v-for循环中使用麻烦，还需用正则去除前缀）

或者换一种写法:
let obj = { icon: '\ue70b'}   （UCS-2编码前缀 \u ）
```
