## 防抖
防抖就像是你按了一个按钮，在释放按钮的过程中，会无数次触发事件，但你只认为最后一次触发的事件是正确的，用来防止重复触发事件。
```javascript
    // 三、防抖，只执行最后一次函数
    // 1.非立即执行 定时器版
    function debounce1(func, wait) {
      let timeout;
      return function () {
        let context = this;
        let args = arguments;
        // 如果timeout保存了定时器，就清除定时器
        if (timeout) clearTimeout(timeout);
        
        timeout = setTimeout(function() {
            func.apply(context, args)
        }, wait);
      }
    }
    // content.onmousemove = debounce1(count, 1000)
    // 2.立即执行 定时器版，触发函数后立即执行，在固定事件内再次触发都会被消除
    // 立即执行版的意思是触发事件后函数会立即执行，然后 n 秒内不触发事件才能继续执行函数的效果。
    function debounce2(func,wait) {
      let timeout;
      return function () {
        let context = this;
        let args = arguments;
        // 这里这是清除了定时器，而不是把timeout变为null
        if(timeout) clearTimeout(timeout);

        let flag = !timeout;
        timeout = setTimeout(() => {
          // 在定时器里才把它变为null
            timeout = null;
        }, wait)

        if(flag) func.apply(context, args)
      }
    }
```