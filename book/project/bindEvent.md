# 事件绑定中的this指向被绑定元素
在做2048的项目时，要给元素同时绑定移动端事件和pc端事件，所以就写了一个事件类，在事件绑定中调用事件类的方法，结果发现在监听函数中获取不到事件类定义的属性，最后发现，原来是this指向已经改变。

```javascript
export const EventUtil = class {
  constructor(move) {
    this.move = move
    this.startX = 0
    this.startY = 0
  }
  handleTouchEvent (event) {
    // console.log('aaa')
    this.move()
    console.log(this.startX)
    switch (event.type){
        case "touchstart":
            this.startX = event.touches[0].pageX;
            this.startY = event.touches[0].pageY;
            break;
        case "touchend":
            var spanX = event.changedTouches[0].pageX - this.startX;
            var spanY = event.changedTouches[0].pageY - this.startY;
  
            if(Math.abs(spanX) > Math.abs(spanY)){      //认定为水平方向滑动
                if(spanX > 30){         //向右
                    this.move('right')
                    console.log(this.move)
                } else if(spanX < -30){ //向左
                    this.move('left')
                }
            } else {                                    //认定为垂直方向滑动
                if(spanY > 30){         //向下
                    this.move('down')
                } else if (spanY < -30) {//向上
                    this.move('up')
                }
            }
  
            break;
        case "touchmove":
            //阻止默认行为
            event.preventDefault();
            break;
    }
  }
  handleKeyEvent(event) {   
    console.log('bbb')
    console.log(this.move)
      switch (event.which) {
        case 37:
          this.move('left')
          break
        case 38:
          this.move('up')
          break
        case 39:
          this.move('right')
          break
        case 40:
          this.move('down')
          break
        default:
          break
      }
  }
  
}
```

所以，就在绑定事件时增加了改变this指向的bind。

```javascript
document.addEventListener("touchstart",  this.eventUtil.handleTouchEvent, { passive: false });  
document.addEventListener("touchstart", this.eventUtil.handleTouchEvent, { passive: false });  
document.addEventListener("touchstart", this.eventUtil.handleTouchEvent, { passive: false });  
document.addEventListener("keydown", this.eventUtil.handleKeyEvent.bind(this.eventUtil), { passive: false });  
```