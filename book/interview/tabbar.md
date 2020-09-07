# 轮播图
### 简单实现——没有动画版本
通过更改`img`的`src`属性实现图片切换。
```html
<div id="container">
  <img src="img/1.jpg" width="100%" height="100%" id="img-items">
  <i id="left"></i>
  <i id="right"></i>
  <ul class="icon-items">
    <li class='icon-circle'></li>
    <li class='icon-circle'></li>
    <li class='icon-circle'></li>
    <li class='icon-circle'></li>
  </ul>
</div>
```
注意这里没给左右切换按钮 和 小圆点设置对应的文字编码。
```css
* {
  margin: 0;
  padding: 0;
  list-style-type: none;
  text-decoration: none;
}
#container {
  width: 500px;
  height: 500px;
  margin: 0 auto;
  position: relative;
}
#img-items {
  width: 100%;
  height: 100%;
}
#left {
  position: absolute;
  top: 45%
}
#right {
  position: absolute;
  top: 45%;
  left:93%;
}
.icon-items {
  position: absolute;
  top: 90%;
  left: 38%;
}
.icon-items #icon-circle{
  float: left;
}
```
```javascript
var container = document.getElementById("container")
// 设置全局变量
var num=1;

// 自动换图
setInterval(function(){
  num++;
  // 到最后一张了，让指针指回第一张
  if(num == 5){
    num = 1;     
  }
  container.src="img/"+num+".jpg" 
},3000)

var left = document.getElementById("left")
var right = document.getElementById("right")

// 点击左箭头换图
left.onclick = function(){
  num--;
  // 第一张再往左，就是最后一张了
  if(num == 0){
    num = 5;     
  }
  container.src="img/"+num+".jpg"   
}

// 点击右键换箭头
right.onclick=function(){
  num++;
  if(num == 5){
    num = 1;     
  }
  container.src="img/"+num+".jpg" 
}

// 点击圆点换图
var allLi = document.getElementsByTagName('ul')[0].getElementsByTagName("li");
for(var i = 0 ; i < allLi.length ; i++){
  // 给每个li元素赋值，每循环一次，i+1;
  allLi[i].index = i;
  allLi[i].onclick=function(){
  // li的索引是从0开始的，所以要+1
    var num = this.index+1;
    container.src="img/"+num+".jpg" 
  }   
}
```
缺点：图片切换不够圆滑
### 用css3实现动画轮播图——无控制按钮
通过动画animation改变`background-position`属性实现轮播图自动切换。
```html
<div class="container"></div>
```
`background-position`决定了轮播图片的起始位置
```css
@keyframes slide {
  0% {
    background-position: 0 0;
  }
  10%, 25% {
    background-position: -600px 0;
  }
  35%, 50% {
    background-position: -1200px 0;
  }
  60%, 75% {
    background-position: -1800px 0;
  }
  85%, 100% {
    background-position: 0 0;
  }
}
.slide-box {
  margin: 0 auto;
  width: 600px;
  height: 400px;
  border: 1px solid #ddd;
  background: url(http://sandbox.runjs.cn/uploads/rs/376/uazzmdfd/bg.png) 0 0 no-repeat;
  animation: slide 8s linear infinite;
}
```