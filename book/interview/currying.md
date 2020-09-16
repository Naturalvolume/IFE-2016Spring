# 函数柯里化
函数的柯里化是指把多个参数的函数通过返回一个新的函数转换成 部分参数 甚至 单个参数的函数，比如下面的加法函数。
```javascript
// 柯里化之前的函数
function add(x, y) {
  return x+y
}
console.log(add(1, 2))  // 3

// 柯里化
function add(x) {
  // 函数中保留 x 参数
  let num = x
  // 返回只需要 y 参数的函数
  return function(y) {
    return num+y
  }
}
let add1 = add(1)
console.log(add1(2))  // 3
```
柯里化一般用闭包实现。
### 柯里化工具函数——简单版
在开发中很多地方都可以用到柯里化，每次都自己写有些麻烦，所以可以把他封装成工具函数。
```javascript
// 柯里化 三个参数的函数
function sum(x, y, z) {
  return x + y+ z
}
```
根据形参数组 arguments 会存储所有的形参，可以把形参单独拿出来。
```javascript
function currying(fn) {
  // 截取 剩余的参数，除了函数 fn
  let args = Array.prototype.slice.call(arguments, 1)
  return function() {
    // 拼接参数
    _args = args.concat(Array.prototype.slice.call(arguments))
    return fn(..._args)
  }
}
let a = currying(sum, 1)
console.log(a(2,3))     // 3

let b = currying(sum, 1, 2)
console.log(b(3))     // 3
```
### 柯里化工具函数——连续调用版
简单版函数已经可以实现柯里化的功能，但是不支持多参数连续调用`fn(a)(b)(c)`，所以利用递归修改简单版函数如下：
```javascript
function processCurrying(fn) {
  // 函数的length属性 是 参数的长度
  let len = fn.length
  let args = Array.prototype.slice.call(arguments, 1)
  return function() {
    _args = args.concat(Array.prototype.slice.call(arguments))
    // 参数长度还不够
    if(_args.length < len) {
      // 直接返回已有部分参数的 函数
      return processCurrying(fn, ..._args)
    } else {
      // 返回结果
      return fn(..._args)
    }
  }
}
let c= processCurrying(sum, 1)
console.log(c(2)(3))
```
### 柯里化性能
- 存取arguments对象通常要比存取命名参数要慢一点
- 一些老版本的浏览器在arguments.length的实现上是相当慢的
- 使用fn.apply( … ) 和 fn.call( … )通常比直接调用fn( … ) 稍微慢点
- 创建大量嵌套作用域和闭包函数会带来花销，无论是在内存还是速度上

总体而言，柯里化性能在某些角度讲并不高，但前端主要的性能瓶颈是在操作DOM节点上，这js的性能损耗基本是可以忽略不计的，所以curry是可以直接放心的使用。

### 优点
- 函数式编程的重要标志
- 灵活性高

### 参考 
[JS函数柯里化](https://www.cnblogs.com/plBlog/p/12356042.html)

