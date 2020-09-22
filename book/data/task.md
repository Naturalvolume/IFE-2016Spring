# async中的微任务
- `await`默认会执行 fulfilled状态的 promise.then()
- `Promise.race`默认会执行状态不为 pending 状态的 promise.then()

所以这两种情况都会产生微任务队列
```javascript
let promise1 = new Promise((resolve, reject) => {
  resolve(1)
})
let promise2 = new Promise((resolve, reject) => {
  resolve(2)
})
let promise3 = new Promise((resolve, reject) => {
  resolve(3)
})

let arr = [promise1, promise2, promise3]
async function difference(arr) {
  for(let key in arr) {
    // 第一个隐藏微任务 arr[0].then()   输出 1 1600778736952
    // 第三个隐藏微任务 arr[1].then()   输出 2 1600778736952
    // 第五个微任务 arr[2].then()     输出 3 1600778736953
    let res = await arr[key]
    console.log(res, Date.now())
  }
}

function handle(promise, time) {
  let hand = new Promise((res, rej) => {
    setTimeout(() => {
      rej("请求超时")
    }, time)
  })
  // 第二个隐藏微任务 Promise.race 中的promise
  return Promise.race([promise, hand])
}

difference(arr)
// 第四个隐藏微任务 handle 函数返回的promise.then   输出 3
handle(promise3, 300).then((res) => {
  console.log(res)
})

// 输出
1 1600778736952
2 1600778736952
3
3 1600778736953
```