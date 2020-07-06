# 类数组
类数组是看起来像数组，实际上不是数组，而是键值为数字的对象，不能使用数组方法的数组，它只有自身定义的`length`属性是跟数组一样的。
```javascript
let arguments = {
  // 键值从 0-n
    '0': 'a',
    '1': 'b',
    '2': 'c',
    // 有 length 属性
    length: 3
};
```
类数组变成数组的几种方式
- Array.from
```javascript
let arr = Array.from(arguments) // ['a','b','c']
```
- ES5中 Array.prototype.slice.call()
```javascript
function test(a,b,c,d) 
   { 
      var arg = Array.prototype.slice.call(arguments,1); 
      console.log(arg); 
   } 
   test("a","b","c","d"); //b,c,d
```
- es6扩展运算符
扩展运算符只能扩展具有`Symbol.iterator`属性的类数组，所以自己定义的`arguments`是不能用扩展运算符直接展开的。

```javascript
// 错误
let arr = [...arguments]  // error

// 正确，形参是类数组
function a(b,c,d) {
  console.log([...arguments]) // 1,2,3
}
a('1','2','3')
```
以上三个方法都是只能在键值严格符合数组下标的情况下起作用。
