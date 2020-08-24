## 节流
节流就像是公交车的发车时间，只能每隔一段时间发出，未到时间就不会发车。
```javascript
// 节流，每隔固定时间执行一次，稀释执行频率
    // 1.用定时器实现，不是立即执行，是在time时间后执行
    var throttle1 = function(fun, time) {
      flag = true
      return function() {
        // 保存上下文
        let context = this
        // arguments 是dom事件
        let args = arguments

        // 没到时间
        if(!flag) return
        flag = false
        setTimeout(() => {
          flag = true
          fun.call(context)
        }, time)
      }
    }

    // throttle在这里调用，保存content的this
    // content.onmousemove = throttle1(count, 10000);
    // 2.用时间戳实现，立即执行
    var throttle2 = function(func, time) {
      let previous = 0;
      return function() {
        // date的内置值，1970.1.1开始的毫秒数
        let now = Date.now();
        let context = this;
        let args = arguments;
        // 不管哪个都会比初始值 0 大
        if (now - previous > time) {
            func.apply(context, args);
            previous = now;
        }
      }
    }
    //content.onmousemove = throttle2(count, 1000);
    // 3.双剑合璧
    function throttle(func, time, type) {
      if(type == 1) var flag = true
      else if(type == 2) var previous = 0
      return function() {
        let context = this
        let args = arguments
        if(type == 1) {
          if(!flag) return
          flag = false           
          setTimeout(() => {
            flag = true
            func.apply(context, args)
          }, time)
        } else if(type == 2) {
          let now = Date.now()
          console.log(now)
          if(now - previous > time) {
            previous = now
            func.apply(context, args)
          }
        }
      }
    }
```