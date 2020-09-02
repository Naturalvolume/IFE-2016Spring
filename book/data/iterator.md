# Iterator和for...of循环
遍历器为数组、对象、map、set等提供了一个同一的接口，来遍历所有不同的数据结构。

### 介绍
**作用**：
- 为各种数据结构提供统一的、简便的访问接口
- 使数据结构成员按某种次序排序
- es6提供新的for...of循环

**Iterator 遍历过程**：
- 创建一个指针对象，指向起始位置
- 每调用一次对象的next方法，就让指针向数据结构后移一位，且返回当前成员的信息（包含value和done两个属性的对象）

value属性是当前成员的值，done属性是一个布尔值，表示遍历是否结束

- 直到指针指向数据结构的结束位置

### 模拟next方法
```javascript
function makeIterator(arr) {
  var nextIndex = 0
  return {
    next: function() {
      return nextIndex < arr.length ? {value: arr[nextIndex++], done: false} : {value: undefined, done: true}
    }
  }
}

var it = makeIterator(['a', 'b'])

console.log(it.next()) 
console.log(it.next()) 
console.log(it.next()) 
```
### 可遍历的
默认Iterator接口部署在数据结构的`Symbol.iterator`属性，所以一个数据结构只要有`Symbol.iterator`属性就可认为是“可遍历的”。

这是因为`Symbol.iterator`是一个函数，是默认的遍历器生成函数，`Symbol.iterator`是一个预定义好的、类型为Symbol的特殊值，所以放在方括号中。
```javascript
const obj = {
  [Symbol.iterator] : function () {
    return {
      next: function () {
        return {
          value: 1,
          done: true
        };
      }
    };
  }
};
```
原生具备Iterator接口的数据结构：
- Array
- Map
- Set
- String
- TypeArray
- arguments
- NodeList

通过执行`Symbol.iterator`方法，返回迭代器对象
```javascript
let arr = ['a', 'b', 'c'];
let iter = arr[Symbol.iterator]();

iter.next() // { value: 'a', done: false }
iter.next() // { value: 'b', done: false }
iter.next() // { value: 'c', done: false }
iter.next() // { value: undefined, done: true }
```

对象没有默认部署Iterator，因为对象哪个属性先遍历是不确定的，需要手动指定，自己在`Symbol.iterator`属性上部署。

### 默认调用Iterator接口的场合
- 解构赋值
- 扩展运算符(...)： 将任何部署了Iterator接口的数据结构，转为数组
- `yield *`: 后面是一个可遍历结构，会调用该结构的遍历器接口
```javascript
let generator = function* () {
  yield 1;
  yield* [2,3,4];
  yield 5;
};

var iterator = generator();

iterator.next() // { value: 1, done: false }
iterator.next() // { value: 2, done: false }
iterator.next() // { value: 3, done: false }
iterator.next() // { value: 4, done: false }
iterator.next() // { value: 5, done: false }
iterator.next() // { value: undefined, done: true }
```

- 数组的遍历会调用遍历器接口
  - for...of
  - Array.from()
  - Map、Set、WeakMap、WeakSet
  - Promise.all()
  - Promise.race()

### for...of循环和其它循环的区别
- 遍历具有`Symbol.iterator`属性的数据
- 直接取到 value 值
- 能用break跳出循环


### Generator函数
`Generator`函数可以最简单的实现`Symbol.iterator`
```javascript
let myIterable = {
  [Symbol.iterator]: function* () {
    yield 1;
    yield 2;
    yield 3;
  }
}
[...myIterable] // [1, 2, 3]

// 或者采用下面的简洁写法

let obj = {
  * [Symbol.iterator]() {
    yield 'hello';
    yield 'world';
  }
};
```
