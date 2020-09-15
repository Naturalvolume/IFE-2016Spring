# async 函数
async 函数是 Generator 函数的语法糖，改进了Generator函数：
- 内置执行器：Generator 函数需要**调用 next 方法**，或者用 **co 模块**，才能真正执行，而await函数自带执行期，自动执行；
- 更好的语义
- 更广的适用性：async函数返回值可以是 Promise 或 原始类型的值（自动转成**立即 resolved 的 Promise 对象**）
async函数 全部执行成功promise状态就为fulfilled，否则为rejected
- await函数后面的异步函数执行完后（**await 命令就是内部then命令的语法糖**），若是promise：
  - 状态为fulfilled，返回resolve的值
  - 状态为reject，且没有错误捕捉机制，则报错，且async函数终止执行
  - 状态为reject，且有错误处理机制，返回错误定位，函数继续执行
  - 状态为pending，则函数阻塞在这里
- 错误处理，当 异步函数出错 或 promise状态变为reject，若没有错误处理机制，那么函数会报错且终止运行，所以需要错误处理机制
  - 放到`try...catch`代码块中
  - 在await函数后的 promise 函数中加入 .catch方法

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

### 使用注意点
- 最好把 await 命令放在`try...catch`代码块中，防止promise运行结果是rejected

```javascript
async function myFunction() {
  try {
    await somethingThatReturnsAPromise();
  } catch (err) {
    console.log(err);
  }
}

// 另一种写法

async function myFunction() {
  await somethingThatReturnsAPromise()
  .catch(function (err) {
    console.log(err);
  });
}
```

- 最好把不存在继发关系的异步操作，同时触发（并发执行）

```javascript
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;

// 写法三
async function dbFuc(db) {
  let docs = [{}, {}, {}];
  let promises = docs.map((doc) => db.post(doc));

  let results = [];
  for (let promise of promises) {
    results.push(await promise);
  }
  console.log(results);
}
```

- 串行执行

```javascript
// 方法一
async function dbFuc(db) {
  let docs = [{}, {}, {}];

  for (let doc of docs) {
    await db.post(doc);
  }
}

// 方法二
async function dbFuc(db) {
  let docs = [{}, {}, {}];

  await docs.reduce(async (_, doc) => {
    await _;
    await db.post(doc);
  }, undefined);
}
```