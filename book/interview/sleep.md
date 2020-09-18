# sleep函数实现
```javascript
// promise 实现
function sleep(timer) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve()
    }, timer)
  })
}

sleep(500).then(callback, errFn)

// async实现
async function fn() {
  await sleep(500)
  callback()
}
```