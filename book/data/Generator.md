# Generator
Generator 函数是 es6 的一种异步编程方案，可以理解为一个状态机，封装了多个内部状态，且执行Generator函数会返回一个遍历器对象，遍历器可以依次遍历函数的每个状态。

Generator函数的写法：
```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
```
上面代码有三个状态：hello，world 和 return 语句（结束执行）；调用函数后，并不执行，而是返回指向内部状态的指针，即遍历器对象。
```javascript
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```
通过调用遍历器的next方法，使指针移向下一个状态（`yield`或`return`），可以理解成分段执行函数，`yield`暂停执行，`next`恢复执行。

### Generator 和 协程
**协程**：一个线程／函数执行到一半，可以暂停执行，将执行权交给另一个线程／函数，等到收回执行权时再恢复执行，即可以并行执行、交换执行权的线程。从内存上看，协程同时存在多个栈，但只有一个栈是运行状态，即协程以多占用内存为代价，实现多任务并行。

Generator函数是“半协程”，因为只有函数多调用者才能将程序多执行权还给Generator函数，但是完全的协程，任何函数都可以暂停协程继续执行。

### Generator的数据交换
next 返回值的 value 属性，是Generator函数向外输出数据；
next 方法还可以接受参数，向 Generator 函数体内输入数据。

```javascript
function* gen(x){
  var y = yield x + 2;
  return y;
}

var g = gen(1);
g.next() // { value: 3, done: false }
g.next(2) // { value: 2, done: true }
```

最后 return 的时候看的不是正常函数要返回的值，而是 next 中传入的值。
### 错误捕捉
Generator 函数内部可以部署错误处理代码，捕获函数体外抛出的错误。
```javascript
function* gen(x){
  try {
    var y = yield x + 2;
  } catch (e){
    console.log(e);
  }
  return y;
}

var g = gen(1);
g.next();
g.throw('出错了');
// 出错了
```

在 Generator 函数体外，使用指针对象的 throw 方法抛出的错误，可以被函数体内的 try...catch 代码块捕获，意味着，出错的代码与处理错误的代码实现了时间和空间上的分离，这对异步编程是非常重要的。