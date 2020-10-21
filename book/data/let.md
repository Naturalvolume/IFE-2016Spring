# let
- 是es6的语法
- 暂时性死区：块级作用域内存在let命令，它所声明的变量就“绑定”这个区域，不再受外部影响

例子一：虽然有全局变量tmp，但是在块级作用域中let又声明了一个局部变量tmp，所以let绑定块级作用域，在let声明前调用tmp，会报错
```javascript
var tmp = 123;

if (true) {
   tmp = 'abc'; // ReferenceError
   let tmp;
}
```

例子二：变量没被声明时，使用typeof不会报错
```javascript
typeof undeclared_variable // "undefined"
```

例子三：形参也存在暂时性死区
```javascript
function bar(x = y, y = 2) {
  return [x, y];
}
bar(); // error
```

暂时性死区的本质就是，**只要一进入当前作用域，所使用的变量就已经存在了，但是不可获取**，只有到声明变量时，才可以获取到变量

- 不能变量提升：变量必须在声明语句之后使用

让let、const变量不再提升，是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为，这种错误在es5很常见。
- 不能重复声明
- 块级作用域：让曾经被广泛使用的匿名立即执行函数表达式（IIFE）不再必要了

`var`是函数作用域，在函数内重复定义会覆盖，而`let`块级作用域则不会。
```javascript
// var 函数作用域
function fun() {
  var n = 10
  if(true) {
    var n = 100
  }
  console.log(n)  // 100  在{}中定义的还是在函数作用域内，不构成块级作用域
}

// let 块级作用域
function fun() {
  let n = 10
  if(true) {
    let n = 100
  }
  console.log(n)  // 10  在{}中定义的let是块级作用域，会把n限制在{}中
}
```

`var`用立即执行函数优化函数作用域的缺陷。
```javascript
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```

- 不是全局变量的属性

# const 
声明一个只读的常量，一旦声明，常量的值就不能改变。
- 改变常量的值会报错
- const一旦声明变量，就必须立即初始化，不能留到以后赋值

```javascript
const foo
// SyntaxError: Missing initializer in const declaration
```
- 只在声明所在的块级作用域内有效
- 不能提升变量
- 存在暂时性死区
- 不是全局变量的属性

### 简单类型和对象类型
- 简单类型：数值、字符串、布尔值
- 复合类型：对象、数组等，变量指向的内存地址，保存的是一个指向实际数据的指针，const只能保证指针是固定的（即总是指向另一个固定的地址）

# es6 声明变量的6种方法
- var(es5)
- function(es5)
- let
- const
- import
- class

var 和 function 声明的全局变量是 顶层对象（window global） 的属性，let const class 则不属于。
# 顶层对象 —— globalThis(es2020)
js语言存在一个顶层对象，提供全局环境（即全局作用域），所有代码都在这个环境中运行，但是顶层对象在各种实现里面是不统一的：
- 浏览器 顶层对象是`window`
- 浏览器和web worker中，`self` 指向顶层对象
- node中，顶层对象是global

若使用this取顶层对象
- 全局环境中，this 返回`顶层对象`；node.js模块 中this返回`当前模块`；es6模块 中this返回`undefined`。
- 严格模式或普通模式下，`new Function('return this')()`，总是会返回全局对象，但是浏览器用了 CSP 后，eval、new Function这些方法都无法使用。

### 取顶层对象
```javascript
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};

// 方法三
// es2020 新提出的 globalThis 作为顶层对象
// 任何环境下，globalThis都是存在的
```

### js 如何实现块级作用域和变量提升
块级作用域：通过let、const声明的变量追加到**词法环境 栈**中，这个块执行结束后，追加到词法作用域的内容被销毁掉

变量提升：在编译阶段把var声明的变量存放到**变量环境（函数上下文）**中

### 作用域相关问题
```javascript
(() => {
  let x, y
  try {
    throw new Error()
  } catch (x) {
    x = 1
    y = 2
    console.log(x)
  }

  console.log(x)
  console.log(y)
})()

// 输出 1 undefined 2
```

`catch`中的`x`形参，相当于在子作用域中新声明的变量，和上一层的`x`不是同一个。