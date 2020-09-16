# 数组去重
### 1. 双重for循环
暴力法，双层循环遇到相同的元素就删掉，同时要修改索引，这个方法直接修改了原数组。
```javascript
function distinct(arr) {
  for(let i=0; i<arr.length; i++) {
    for(let j = i+1; j<arr.length; j++) {
      // 要用完全相等，否则数字和字符会分辨不出来
      if(arr[i] === arr[j]) {
        arr.splice(j, 1)
        // 改变了数组，索引也要改变
        j--
      }
    }
  }
  return arr
}
let arr = [1,1,2,3]
console.log(distinct(arr))
```
**限制** ：`===`无法去重NaN和对象。
### 2. for循环和indexOf
用indexOf的属性可以实现在任意遍历中去除重复元素。
```javascript
let arr = [1,1,2,3,'3','4','5',NaN, NaN, /a/,/a/, {a:1},{}]

function distinct(arr) {
  let newarr = []
  for (let i = 0; i < arr.length; i++) {
    if(arr.indexOf(arr[i]) == i) newarr.push(arr[i])
  }
  return newarr
}

console.log(distinct(arr))  //  [1, 2, 3, "3", "4", "5", /a/, /a/, {…}, {…}]
```
**限制** ：`indexOf`无法识别出NaN，所以会直接忽略掉`NaN`，另外它的底层实现是`===`判断，所以indexOf 也无法去重 对象。
### 3. filter和indexOf
用数组的高阶函数filter和indexOf过滤重复元素。
```javascript
function distinct(arr) {
  let newarr = arr.filter((value, index) => {
    return arr.indexOf(value) == index
  })
  return newarr
}
```
**限制** ：`indexOf`无法识别出NaN，所以会直接忽略掉`NaN`，另外它的底层实现是`===`判断，所以indexOf 也无法去重 对象。到 NaN 元素
### 4. Set
Set是es6新增的数据结构，不存储重复元素。
```javascript
function distinct(arr) {
  let set = new Set(arr)
  return [...set]
}
```
**限制**：对象不去重，如`/a/`、`{}`等

**优势**：虽然 Set 底层也是用`===`实现的，但是 Set 还是能去重 NaN
### 5. Object键值对
利用object去重，把数组元素当作对象的键值，最后用`Object.keys(obj)`返回对象的键值数组。
```javascript
let arr = [1,1,2,3,'3','4','5']
function distinct(arr) {
  let obj = {}
  for(let i=0; i<arr.length; i++) {
    if(!obj[arr[i]]) {
      obj[arr[i]] = arr[i]
    }
  }
  return Object.keys(obj)
  // 若是返回值的集合 return Object.values(obj)
  // 返回 [1, 2, 3, "4", "5"]
}
// 不需要用展开字符展开
console.log(distinct(arr))  // ["1", "2", "3", "4", "5"]
```
**限制**：因为对象的键是字符类型，所以用键值数组输出的全部都是字符类型元素，并且数字`3`和字符`'3'`会被识别成同一个元素。

**优势**：可以去重NaN和对象，因为`NaN.toString()`会是`NaN`字符串。
在键值中加入类型判断，然后去重解决无法识别数字和字符的问题：
```javascript
let arr = [1,1,2,3,'3','4','5','6',33,NaN, NaN, /a/,/a/, {a:1},{}]
function distinct(arr) {
  let obj = {}
  for(let i=0; i<arr.length; i++) {
    if(!obj[typeof arr[i]+arr[i]]) {
      obj[typeof arr[i]+arr[i]]= arr[i]
    }
  }
  return Object.values(obj)
  // 若是返回值的集合 return Object.values(obj)
  // 返回 [1, 2, 3, "4", "5"]
}

console.log(distinct(arr))  
// [1, 2, 3, "3", "4", "5", "6", 33, NaN, /a/, {…}]
```
**限制**：对象类型一律按照`object[object Object]`处理，所以不管对象一不一样，都只保留第一个出现的元素。
### 6. sort排序
用sort先对数组排序，然后循环判断相邻元素是否相同，相同就删掉重复元素，直接在原数组上更改。
```javascript
function distinct(arr) {
  arr.sort((a,b)=> a-b)
  for (let i=1; i<arr.length; i++) {
    if(arr[i] === arr[i-1]) {
      // 去除重复元素
      arr.splice(i, 1)
      // 恢复索引
      i--
    }
  }
  return arr
}
```
**限制**：无法去重NaN和对象