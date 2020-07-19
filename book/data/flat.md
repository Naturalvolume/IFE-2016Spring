# 数组扁平化
### 1. 递归 ＋ Array.isArray判断
```javascript
let arr = [1,2,[3,4],[[5,6]]]
function flatten(arr) {
  let res = []
  arr.forEach((value, index) => {
    if(Array.isArray(value)) {
      res = res.concat(flatten(value))
    } else {
      res.push(value)
    }
  })
  return res
}
console.log(flatten(arr)) 
```
优点：容易想到
缺点：无法展开对象
### 2. Array.prototype.flat(Infinity)
```javascript
let arr = [1,2,[3,4],[[5,6]]]
console.log(arr.flat(Infinity)) 
```
优点：直接利用数组的原型方法扁平化
### 3. 高阶函数 reduce 实现
reduce 和 concat 均不改变原数组，而是返回新数组；注意 reduce 要传入初始值 []，否则将不能使用数组方法。
```javascript
function flatten(arr) {
   return arr.reduce((pre, cur) => {
     return pre.concat(Array.isArray(cur) ? flatten(cur) : cur)
   }, [])
} 
console.log(flatten(arr)) 
```
### 4. 高阶函数 some 实现
some 用来判断数组中是否还有数组元素，只要还有一个数组元素，就会返回 true；展开字符每次只能展开数组的一层，比如`...[1,[2, [3]]]`转换成`[1,2,[3]]`，所以需要循环用 some 判断是否已展开到最后一层。
```javascript
function flatten(arr) {
   while(arr.some(item => Array.isArray(item))) {
     arr = [].concat(...arr)
   }
   return arr
} 
console.log(flatten(arr)) 
```

