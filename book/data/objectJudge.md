# 对象
### 一、判断对象是否为空
1. Object.getOwnPropertyNames()
返回对象中属性名生成对数组
```javascript
var obj = {}
Object.getOwnPropertyNames(obj).length === 0; // true
```
2. 把对象内容转换成字符串
不能用`toString`方法转换对象为字符串，因为`{}.toString()`，输出`[object Object]`
```javascript
var obj = {};
var a = JSON.stringify(obj);
a === "{}"; // true
// a === {}; // false
```
3. for...in循环
`for...in`循环能用来遍历对象类型，但是`for...of`只能用来遍历具有`Symbol.iterator`属性的对象，比如`Array、Set、Map`等。
```javascript
var obj = {};

function judgeObj(obj) {
  for(let key in obj) {
    return false
  }
  return true
};

judgeObj(obj); // true
```
4. Object.keys(obj)
`Object.keys(obj)`是`Object`的构造函数方法，返回`obj`的键值，它与`Symbol.iterator`对象的`keys`是不一样的：
```javascript
let set = new Set()
set.add(1)
console.log(Object.keys(set))       // []   空数组，无键值信息
console.log(set.keys())       // SetIterator {1}   键值信息
```
判断是否为空数组
```javascript
var obj = {}
Object.keys(obj).length === 0; // true
// 或者用值
Object.values(obj).length === 0; // true
```
### 二、对象的常用函数
- Object.prototype.hasOwnProperty(prop)
判断对象自身属性，而不是原型链继承到的属性。
- Object.create(parent, descr) (ES5)
创建新对象，并设置它的原型（parent），和值（descr）。
```javascript
var parent = {hi: 'Hello'};
var o = Object.create(parent, {
    prop: {
        value: 1
    }
});
o.hi; // 'Hello'
// 获得它的原型
Object.getPrototypeOf(parent) === Object.prototype; // true 说明parent的原型是Object.prototype
Object.getPrototypeOf(o); // {hi: "Hello"} // 说明o的原型是{hi: "Hello"}
o.hasOwnProperty('hi'); // false 说明hi是原型上的
o.hasOwnProperty('prop'); // true 说明prop是原型上的自身上的属性。
```
- Object.getPrototypeOf(obj)
获取对象`obj`的原型。