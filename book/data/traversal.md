# js 中的几种遍历方式
### Array.prototype.map()
js 的一个高阶函数，传入函数对数组中的每个元素做同样的操作，然后返回改变后的新数组，不改变原数组。
```javascript
let arr = [1,2,3,4]
let narr = arr.map((item, index) => {
  // break  错误
  // continue   错误
  return item*2
})
console.log(narr)   // [2, 4, 6, 8]
```
- map常用来需要更改数组中每个元素的操作
- 返回新数组
- 除了异常，不能用 break continue 中止或跳出循环
- 可以和其他高阶函数形成链式调用
### Array.prototype.forEach
- 可替代 for 循环遍历数组
```javascript
const items = ['item1', 'item2', 'item3'];
const copy = [];

// before
for (let i=0; i<items.length; i++) {
  copy.push(items[i]);
}

// after
items.forEach(function(item){
  copy.push(item);
});
```

- 没有return，不返回新数组，默认返回 undefined
- 不能被链式调用
- 除了异常，不能中止或跳出 forEach 循环，若要中断循环，可以（1）用try监视代码，在需要中断的地方抛出异常；（2）官方推荐，用some（一个满足，为true） every（所有满足，为true） find（返回第一个满足的） findIndex（返回第一个满足的索引）替代。
- 可过滤打印稀疏数组
```javascript
const arraySparse = [1,3,,7];
let numCallbackRuns = 0;

arraySparse.forEach(function(element){
  console.log(element);
  numCallbackRuns++;
});

console.log("numCallbackRuns: ", numCallbackRuns);

// 1
// 3
// 7
// numCallbackRuns: 3
```
### for...in（不推荐使用）
不仅可以遍历数组，还可以遍历**对象**，注意，这里遍历的是属性值。
```javascript
for (props in obj)
{
    在此执行代码
}
```
### for...of
for..of循环内部调用的是数据结构的Symbol.iterator方法，具有Symbol.iterator迭代属性的，可用这个方法遍历，如 字符串、数组、类数组、Map、Set、generator。所以不能遍历对象，除非给对象加上一个 iterable 属性。
```javascript
for (item in iterator)
{
    在此执行代码
}
```

for...of 遍历的是数组中的每个元素，而不是下标。
