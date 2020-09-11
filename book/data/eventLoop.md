# 事件循环机制
### nodejs 事件循环
nodejs的eventLoop 和 浏览器eventLoop的异同
- nodejs的微任务在各阶段之间执行，浏览器的微任务在当前宏任务执行完毕时执行
- nodejs新增了promise.nextTick 和 setImmediate
  - promise.nextTick是微任务，在每阶段中间执行，且会将当前的所有微任务执行完，才进入下一个阶段
  - setImmediate 相当于 setTimeout(fn, 0)，因为计数器有延时，setTimeout(fn, 0)不会真的是0秒后就执行，而是将近1秒后执行
- nodejs中setImmediate虽然在事件循环中是在setTimeout之后执行的，但是由于计时器有延时，所以setImmediate和setTimeout(fn, 0)的执行顺序不一定

参考：
[JavaScript 运行机制详解：再谈Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)
[SetTimeout、SetInterVal、setImmediate和process.nextTick的理解](https://juejin.im/post/6844903571331219464)
