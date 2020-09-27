# 一些比较难的面试题
#### 案例一：
```javascript
var a = 0, b = 0
function A(a) {
  A = function(b) {
    alert(a + b++)
  }
  alert(A++)
}
A(1)
A(2)
```

闭包的优点：保存或保护

#### 案例二：
```javascript
function Foo() {
  getName = function() {
    console.log(1)
  }
  return this
}
Foo.getName = function() {
  console.log(2)
}
Foo.prototype.getName = function() {
  console.log(3)
}
var getName = function() {
  console.log(4)
}
function getName() {
  console.log(5)
}

Foo.getName()
getName()
Foo().getName()
getName()
new Foo.getName()
new Foo().getName()
new new Foo().getName()
```

输出 2 4 1 1 2 3 3

`new`的这几个涉及到了运算符优先级问题，其中
`new Foo()` == 


#### 案例二：
```javascript
async function async1() {
  console.log('async1 start')
  await async1()
  console.log('async1 end')
}
async function async2() {
  console.log('async2')
}
console.log('script start')
setTimeout(function() {
  console.log('setTimeout')
}, 0)
async1()
new Promise(function(resolve) {
  console.log('promise1')
  resolve()
}).then(function() {
  console.log('promise2')
})
console.log('script end')
```
注意：`await`是默认执行`promise`的，所以也是微任务。

#### 案例三：
```javascript
function A() {
  alert(1)
}
function Func() {
  A = function() {
    alert(2)
  }
  return this
}
Func.A = A
Func.prototype = {
  A: () => {
    alert(3)
  }
}
A()   // '1'
Func.A()  // '1'
Func().A()  // '2'
// Func.A 是等于原来的A函数，而不是更改过后的
new Func.A()  // 错了，答案是 '1'
new Func().A()  // '3'
new new Func().A()  // 错了，答案是会报错，因为 箭头函数不能被new
```

箭头函数不能被`new`，因为它没有prototype等属性

#### 案例四：
```javascript
var x = 2 
var y = {
  x: 3, // 3*2 = 6
        // 24
        // 24 * 2 = 48
  z: (function(x) {

    this.x *= x
    x += 2
    return function(n) {
      this.x *= n
      x += 3
      console.log(x)
    }
  })(x)
}
var m = y.z
m(4)      // 7
y.z(5)    // 10
console.log(x, y.x)   // 16 15
```

#### 案例五：
```javascript
// 当a等于什么的时候，满足下面的条件，输出 1

// 方法一：用toString 或 valueOf 方法
var a = {
  i: 0,
  // 换成valueOf也是一样的
  toString() { 
    return ++this.i   // 每次都会默认的调用 a.toString()
  }
}

// 方法二：用Object.defineProperty方法，相当于劫持 window.a 
// 定义i作为每次的返回值
var i = 0
Object.defineProperty(window, 'a', {
  get() {
    return ++i
  }
})

// 方法三：数组的 shift 方法
var a = [1, 2, 3]
a.toString = a.shift

// 执行
if(a == 1 && a == 2 && a == 3) {
  console.log(1)
}
```

- 对象 == 字符串：对象`toString()`变为字符串
- null == undefined: 相等，但是它们和其它值都不相等
- NaN == NaN: false
- 剩下的都是转换为数字，比如
  - 对象先调用`toString()`，再`Number()`


#### 案例六：
```javascript
var x = 0, y = 1
function fn() {
  x += 2
  fn = function(y) {
    console.log(y * (--x))
  }
  console.log(x, y)
}
fn(3)
fn(4)
console.log(x, y)
```

#### 案例七：
```javascript
setTimeout(() => {
  console.log(1)
}, 20)
console.log(2)
setTimeout(() => {
  console.log(3)
}, 10)
console.log(4)
console.log('AA')
for(let i = 0; i < 90000000; i++) {
  // do something
}
console.timeEnd('AA')   // AA 79ms 左右
console.log(5)
setTimeout(() => {
  console.log(6)
}, 8)
console.log(7)
setTimeout(() => {
  console.log(8)
}, 15)
console.log(9)
```

2 4 aa aa 5 7 9 3 1 6 8