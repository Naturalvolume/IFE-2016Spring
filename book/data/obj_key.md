# 对象作为键值时
首先，明确一点 **对象的键值在运行时默认会调用它的`toString()`方法**
```javascript
let obj = {}
let a = {}
let b = {}

obj[a] = 'kathy'
obj[b] = 'beautiful'

console.log(obj[a])   // beautiful
console.log(obj)      
//  { 
//    [object Object] beautiful
// }
```

当对象作为键值的时候，隐式调用`toString()`方法，键值为`[object Object]`，所以后来会被覆盖掉。