# 无限累加函数
要实现无限累加的函数，需要知道一个知识点：每次`console.log`的时候都会调用输出对象的`toString`方法，下面直接看代码。
```javascript
function add(a) {
    function sum(b) { // 使用闭包
      console.log(b)
      a = a + b; // 累加
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