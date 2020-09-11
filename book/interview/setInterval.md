### setTimeout
setTimeout(callback, time) 是处理异步的一种传统方式，在 time 毫秒后执行回调函数 callback。

**注意的点**
- setTimeout是宏任务
- 最后执行的时间会 大于等于 time 
- 会出现回调地狱
- 清除定时器用 clearTimeout
- html5规定time最小值不得低于4ms，若低于这个值就会自动增加，并且，对于**dom的变动（尤其是涉及到页面重新渲染的部分）**，通常不会立即执行，而是每16ms执行一次（此时用**requestAnimationFrame()**效果更好）

### setInterval
setInterval(callback, time) 是间隔 time 时间循环执行回调函数，除非用 clearInterval 清除setInterval

**setInterval的缺点**：
- 不关心回调函数是否还在运行
在某些情况下，回调函数执行的时间大于定义的时间间隔，比如 客户端每隔5s对服务器进行轮询，服务器无响应或其他因素会阻止请求按时完成，结果导致返回一串队列请求。
- 忽视错误
代码中出现错误，setInterval不会中止执行而是继续执行错误的代码。
- 缺乏灵活性
只要不手动中止，会一直执行下去，需要一个如执行次数一样的参数。

### 用setTimeout实现setInterval
总体思路是用递归，在setTimeout中递归调用setTimeout

```javascript
// 有问题版本
function timeInterval(time) {
  setTimeout(function(){
    console.log('success')
  }, time)
  // setTimeout是要在下一次宏事件中执行的
  // 这样会造成内存溢出
  timeInterval(1000)
}
timeInterval(5000)

// 正确的版本，在setTimeout中调用setTimeout
function timeInterval(time) {
  setTimeout(function(){
    console.log('success')
    timeInterval(1000)
  }, time)
  
}
timeInterval(5000)

// 添加清除定时器语句，防止内存溢出
function timeInterval(time) {
  let timer = setTimeout(function(){
    console.log('success')
    timeInterval(1000)
    clearTimeout(timer)
  }, time)
  
}
timeInterval(5000)

// 加上执行次数参数
function interval(func, w, t){
    var interv = function(){
      // 为定义执行次数t，就会一直执行
        if(typeof t === "undefined" || t-- > 0){
            setTimeout(interv, w);
            try{
                func.call(null);
            }
            catch(e){
                t = 0;
                throw e.toString();
            }
        }
    };

    setTimeout(interv, w);
};
interval(()=>{console.log('success')}, 1000,10)
```

### setTimeout的实现原理
chrome中除了正常的消息队列外，还有一个**延迟消息队列**，用来维护要延迟执行的任务列表，包括定时器和chromium内部一些需要延迟执行的任务。

当通过js创建一个定时器，渲染进程会将该定时器的回调任务包装成有基本信息的回调任务添加到延迟队列中：
```javascript
// 原有的回调函数
function foo(){
  console.log("test")
}
var timeoutID = setTimeout(foo,100);

// 包装成包含 回调函数、当前发起时间、延迟执行时间等
struct DelayTask{
  int64 id；
  CallBackFunction cbf;
  int start_time;
  int delay_time;
};
DelayTask timerTask;
timerTask.cbf = foo;
timerTask.start_time = getCurrentTime(); //获取当前时间
timerTask.delay_time = 100;//设置延迟执行时间
```

把包装好的回调函数添加到延迟执行队列中，由用来处理延迟执行任务的函数`ProcessDelayTask`处理，`ProcessDelayTask`函数会根据发起时间和延迟时间计算出到期的任务，然后依次执行到期的任务。

**setTimeout的注意事项**：
- 回调函数执行的时间不一定是定时时间
- this指向window
- 嵌套调用5次后，系统会设置最短执行时间间隔为**4ms**，是因为chrom源码中当定时器被嵌套5次以上，系统会判断该函数方法被阻塞了，然后设置时间间隔为**4ms**
- 延迟执行时间的最大值：chrome、safari、firefox都是以32比特存储延时值的，32比特最大能存放的数字是 2147483647 毫秒，所以当设置的延迟值大于2147483647 毫秒（大约 24.8 天）时就会溢出，导致定时器被立即执行