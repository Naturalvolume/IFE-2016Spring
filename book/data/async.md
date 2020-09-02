# async 函数
async 函数是 Generator 函数的语法糖，改进了Generator函数：
- 内置执行器：Generator 函数需要调用 next 方法，或者用 co 模块，才能真正执行，而await函数自带执行期，自动执行；
- 更好的语义
- 更广的适用性：值可以是 Promise 或 原始类型的值（自动转成立即 resolved 的 Promise 对象）
- 返回值是Promise：可以用then方法添加回调函数，await后的内容成功执行，promise的状态就是 fulfilled，执行失败，promise的状态是rejected

```javascript
async function f() {
  return 'hello world';
}

// return 语句返回的值，成为then方法回调函数的参数
f().then(v => console.log(v))
// "hello world"
```

### 实现原理
async函数就是将 Generator 函数和自动执行器包装在一个函数里。
