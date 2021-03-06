# 高阶函数之返回函数的函数
### 一、类型判断函数
返回函数的函数也是高阶函数。
```javascript
// 常规实现
function isType(type) {
  return function (obj) {
    return Object.prototype.toString.call(obj) === '[object ' + type + ']'
  }
}
console.log(isType('String'))       // function (obj){}
console.log(isType('String')('123'))    // true

// 箭头函数实现
let isType = type => obj => {
  return Object.prototype.toString.call(obj) === '[object ' + type + ']'
}
console.log(isType('String'))   // obj => {}
console.log(isType('String')('123'))  //true
```
### 二、无限累加函数
要实现无限累加的函数，需要知道一个知识点：每次`console.log`的时候都会调用输出对象的`toString`方法，下面直接看代码。
```javascript
function add(a) {
  // 使用闭包，让形参 a 在这个作用域里面不释放
    function sum(b) { 
      a = a + b; // 累加
      // 每次都返回 sum 函数
      return sum;
    }
    sum.toString = function() { // 重写toString()方法
      return a;
    }
    return sum; // 返回一个函数
}

console.log(add(1))   // f 1
console.log(add(1)(2))  // 2
                        // f 3
console.log(add(1)(5)) // 5
                        // f 6
```
这个代码的过程是这样的：
1. 当`console.log(add(1)) `时
先是形参`a=1`，`sum`函数不执行，最后return`sum`函数，`console.log`时，调用返回的sum对象的`toString`方法，返回`a`。
```javascript
// 形参 a=1
function add(a) {
  // 不执行
    function sum(b) { 
      console.log(b)
      a = a + b;
      return sum;
    }
    sum.toString = function() { 
      return a;
    }
  // 返回一个函数，add(1)之后变成了 sum
    return sum; 
}
```
2. 当`console.log(add(1)(2)) `时
`add(1)`返回了`sum`函数，且给`a`赋值1，所以接下来执行`sum(2)`，打印出b的值2，a累加为3，返回`sum`函数，最后`console.log(sum)`自动调用`toString()`方法打印出3

### 无限累加函数——进阶版
实现 
`add(2,3,4) = 9`
`add()()()(2)(3,4) = 9`
```javascript
function add() {
  let initial
  if(arguments.length > 1) {
    initial = [...arguments].reduce((accu, curr) => {
      return accu + curr
    }, 0)
  } else if(arguments.length == 1) {
    initial = arguments[0]
  } else {
    initial = 0
  }

  function helper() {
    let args = [...arguments]
    initial = args.reduce(function(accu, curr) {
      return accu + curr
    }, initial)
    return helper
  }
  helper.toString = function() {
    return initial
  }

  return helper
}

console.log(add(1))
console.log(add())
console.log(add()(2,3))
console.log(add(1)(2,3)(4))
console.log(add(2, 3)(4, 5))
console.log(add(1)(2))
```