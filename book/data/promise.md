### 1. 基本类型
```javascript
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .then(console.log)
```
Promise.resolve 方法的参数如果是一个原始值，或者是一个不具有 then 方法的对象，则 Promise.resolve 方法返回一个新的 Promise 对象，状态为resolved，Promise.resolve 方法的参数，会同时传给回调函数。
then 方法接受的参数是函数，而如果传递的并非是一个函数，它实际上会将其解释为 then(null)，这就会导致前一个 Promise 的结果会穿透下面。

输出1
### 2.红灯三秒亮一次，绿灯一秒亮一次，黄灯2秒亮一次；如何让三个灯不断交替重复亮灯？
用递归实现 和 promise 实现
```javascript
function red() {
    console.log('red');
}
function green() {
    console.log('green');
}
function yellow() {
    console.log('yellow');
}

var light = function (timmer, cb) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            cb();
            resolve();
        }, timmer);
    });
};

var step = function () {
    Promise.resolve().then(function () {
        return light(3000, red);
    }).then(function () {
        return light(2000, green);
    }).then(function () {
        return light(1000, yellow);
    }).then(function () {
        step();
    });
}

step();
```
[处理循环异步的方法](https://www.cnblogs.com/xiaole9924/p/11841629.html)

### 3.promise.all()
promise.all() 提供了一个异步任务并发执行，并判断所有任务是否成功执行的方法。
```javascript
const promise1 = Promise.resolve(3);
const promise2 = 42;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo');
});

Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
// expected output: Array [3, 42, "foo"]
```
只要有一个 失败reject，promise就失败，全部都 成功resolve，才会执行成功的回调。
### 4.微事件宏事件执行顺序
```javascript
console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})
```
输出结果：1 7 6 8 2 4 3 5 9 11 10 12
每遇到 setTimeout 就新建一个宏事件队列，所以本例中的两个setTimeout不在同一个宏事件中执行，而是会先执行完setTimeout中的微事件 再 执行下一个setTimeout。
### 5.promise中的错误能够被定位吗
```javascript
```
### 6.如何处理循环的异步操作
```javascript
```
### 7.使用Promise实现串行
```javascript
```
### 8. promise 的异常捕获
有两种方式：
- 在 .then() 中定义第二个函数，捕获错误
- 用promise .catch() 全部捕获错误，.catch放的位置会影响能否捕获到错误，因为promise会按顺序执行
```javascript
// .then中定义两个函数
let a = new Promise((resolve) => {
  console.log('success')
  resolve(2)
}).then((res) =>{
  console.log(res)
  resolve(3)
}, (err) =>{
  console.log(err)
}).then((res) => {
  console.log(res)
}, (err) =>{
  console.log(err)
})
// 输出 success
    // 2
    // ReferenceError: resolve is not defined at demo.html:29

// 在catch中捕获
let a = new Promise((resolve) => {
  console.log('success')
  resolve(2)
}).then((res) =>{
  console.log(res)
  resolve(3)
}).then((res) => {
  console.log(res)
}).catch((err) =>{
  console.log(err)
})
// 输出 success
    // 2
    // ReferenceError: resolve is not defined at demo.html:29
```
当同时定义 .then 的err函数 和 .catch 时，会优先使用 .then 捕获，这跟代码的定义位置也有关系，若 .catch 定义在 .then 后面，就无法捕获错误。
```javascript
let a = new Promise((resolve) => {
  console.log('success')
  resolve(2)
}).catch((err) =>{
  console.log(err)
}).then((res) =>{
  console.log(res)
  resolve(3)
}).then((res) => {
  console.log(res)
})
// 错误被浏览器捕获，而不是被 catch 函数捕获
```
[深刻理解Promise系列(四):catch](https://www.jianshu.com/p/1c829edec185)
```javascript
```
```javascript
```