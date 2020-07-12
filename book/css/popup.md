# css实现弹出框
### 弹出框的显示与关闭
只使用css实现弹出框，需要借助`a`标签的锚点 与 被连接元素的 `:target` 伪类选择器。

页面布局由一个打开弹窗的`a`标签和 弹窗 组成。
```html
<a href="#popup">删除</a>
<div id="container">
  <a href="#">关闭</a>
</div>
```
css主要是弹窗的居中布局，在这里用的是 固定定位 + top bottom left right 均为0 + margin:auto。

注意：
- 当没有 `margin:auto`时，是以 top、left 位置优先，弹出框会在上部左部。
- 弹出框的显示和隐藏，在页面功能简单的时候有三种实现方法：
  - `display:none` + `display:block`：改变生成的dom树，每次都会重排，且不能添加`transition`效果；
  - `opacity: 0` + `opacity: 1`：opacity只是把元素的颜色变透明了，但是绑定在它身上的事件还是会被触发的；
  - `visibility:hidden` + `visibility:visible`：visibility把元素隐藏起来，绑定在它身上的事件也不会被触发。
```css
  #container {
    width:300px;
    height:300px;
    position:fixed;
    top:0;
    bottom:0;
    left:0;
    right:0;
    margin:auto;
    background: rgba(114,224,24);
    visibility:hidden;
  }
  #container:target {
    visibility:visible;
  }
```
### 弹出框显示3秒后自动关闭
#### 1. 用 transition + opacity 实现
先给 container 增加`opacity`属性
```css
#container {
  visibility: visible;
  opacity: 1;
}
```
当锚点改变时，把`opacity`设为0，用`transition`平缓过渡`opacity`属性
```css
#container:target {
  visibility: hidden;
  opacity: 0;
  transition: opacity 3000ms ease-in;
}
```
这里注意：虽然`transition`是即时变换，但不能直接把` transition-delay`直接设为0，会失效。

最后给 container 添加上动画完成后触发的事件`transitionend`，修改页面的 hash 值。
```javascript
let container = document.getElementById('container')

container.addEventListener('transitionend', function() {
  console.log('success')
  window.location.hash = ''
},false)
```
**实现效果**：点击 删除 后，hash值变为`#container`,弹出框出现，3秒内缓慢消失，hash值也变为`#`。
#### 2. animation动画
animation 可以比 transition 定义更丰富的动画效果。
先给 container 增加`opacity`属性
```css
#container {
  opacity: 1;
}
```
锚点改变时执行动画，通过在`keyframes`中把`opacity:1`的时间间隔设置的长一点，让弹出框不那么快的消失。
```css
 #container:target {
    visibility:visible;
    opacity: 0;
    animation: changePop 3s linear;
  }
  @keyframes changePop {
    0% { opacity:1}
    25% { opacity:1}
    50% { opacity:1}
    75% { opacity:1}
    100% { opacity:0}
  }
```
最后，监听`nimationend`完成事件，改变锚点。
```javascript
let container = document.getElementById('container')

container.addEventListener('animationend', function() {
  console.log('success')
  window.location.hash = ''
},false)
```