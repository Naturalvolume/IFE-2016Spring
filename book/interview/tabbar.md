# 轮播图
### 简单实现——没有动画版本
通过更改`img`的`src`属性实现图片切换。
```html
<div id="fade">
  <img src="img/1.jpg" width="100%" height="100%" id="lunbo">
  <i class='icon-jiantouzuo now-left' id="left"></i>
  <i class='icon-jiantouyou now-right' id="right"></i>
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
#fade {
  width: 500px;
  height: 500px;
  margin: 0 auto;
  position: relative;
}
.now-left {
  position: absolute;
  top: 45%
}
.now-right {
  position: absolute;
  top: 45%;
  left:93%;
}
.icon-items {
  position: absolute;
  top: 90%;
  left: 38%;
}
.icon-items li{
  float: left;
}
```
```javascript
var lb = document.getElementById("lunbo")
        var num=1;


        // 自动换图
        setInterval(function(){
            num++;
            if(num == 5){
                num = 1;     
            }
            lb.src="img/"+num+".jpg" 
            // console.log(num)
        },3000)
        var left = document.getElementById("left")
        var right = document.getElementById("right")

        // 点击左箭头换图
        left.onclick=function(){
            num--;
            if(num == 0){
                num = 1;     
            }
            lb.src="img/"+num+".jpg" 
   
        }

        // 点击右键换箭头
        right.onclick=function(){
            num++;
            if(num == 5){
                num = 4;     
            }
            lb.src="img/"+num+".jpg" 
        }


        // 点击圆点换图
        var allLi = document.getElementsByTagName('ul')[0].getElementsByTagName("li");
        for(var i = 0 ; i < allLi.length ; i++){
            // 给每个li元素赋值，每循环一次，i+1;
            allLi[i].index = i;
            allLi[i].onclick=function(){
                // li的索引是从0开始的，所以要+1
                var num = this.index+1;
                lb.src="img/"+num+".jpg" 
            }   
        }
```