### setTimeout
setTimeout(callback, time) 是处理异步的一种传统方式，在 time 毫秒后执行回调函数 callback。

**注意的点**
- setTimeout是宏任务
- 最后执行的时间会 大于等于 time 
- 会出现回调地狱
- 清除定时器用 clearTimeout

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
