## 数据的深度克隆
- 深度克隆基本数据类型和 Date、RegExp、Array、Object 类型。
- 遍历Object类型数据时，还需要考虑`Symbol`数据类型，所以不能通过`Object.keys`获取键名 或 `for...in`遍历，而是通过`getOwnPropertyNames`和`getOwnPropertySymbols`函数将键名组合成数组，然后进行遍历，这种的实现方式可参考[Object深度克隆](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=180#/detail/pc?id=3178)。

```javascript
// 一、深度克隆
// 给Date对象添加原型方法克隆
Date.prototype.clone = function() {
  // this指向date， this.valueOf()输出date的内部表示，即从1970.1.1以来的毫秒数
  return new Date(this.valueOf())
}
// 给RegExp添加原型方法克隆
 

// let regex = new RegExp('/111/','igm')
RegExp.prototype.clone = function() {
  // valueOf会把正则转换成字面量形式  //111//igm
  // /\/abc/  会自动加转义字符 \
  let pattern = this.valueOf()
  // 保存修饰符
  let flags = ''
  // 判断是否有这个修饰符 
  flags += pattern.global ? 'g' : ''
  flags += pattern.ignoreCase ? 'i' : ''
  flags += pattern.multiline ? 'm' : ''
  // pattern.source 是只有 //111// 正则字面亮部分
  return new RegExp(pattern.source, flags)
}

function cloneobj(obj) {
  // 截取 [object Array] 中的Array，slice可以有负值
  // 一定要注意后面的 -1 表示不要 ']'
  const type = Object.prototype.toString.call(obj).slice(8, -1)

  let res
  switch(type) {
    // 包装对象和基本类型
    case 'Number':
      res = obj - 0
      break
    case 'String':
      res = obj + ''
      break
    case 'Bollean':
      res = obj
      break
    // 数组
    case 'Array':
      res = deepclone(obj)
      break
    // 对象
    case 'Object':
      res = deepclone(obj)
      break
    // 克隆时间对象
    case 'Date':
      res = obj.clone()
      break
    // 克隆正则表达式对象
    case 'RegExp':
      res = obj.clone()
      break
    // 克隆undefined null等
    default:
      res = obj
  }
  return res
}

function deepclone(obj) {
  // 不需要判断是null，因为Object.prototype.toString.call可以判断出null
  // if(obj == null) return null
  let newObj = obj instanceof Array ? [] : {}
  for(let key in obj) {
    // 注意：要判断key是否是obj上的属性
    if(obj.hasOwnProperty(key)) {
      // 注意啦 这里直接用typeof判断是否为对象即可
      newObj[key] = typeof obj[key] == 'object' ? deepclone(obj[key]) : obj[key]
    }
  }
  return newObj
}
```
#### Object.prototype.toString.call()
`Object.prototype.toString.call()`能够准确的判断出数据的类型
```javascript
Object.prototype.toString.call({})              // '[object Object]'
Object.prototype.toString.call([])              // '[object Array]'
Object.prototype.toString.call(() => {})        // '[object Function]'
Object.prototype.toString.call('seymoe')        // '[object String]'
Object.prototype.toString.call(1)               // '[object Number]'
Object.prototype.toString.call(true)            // '[object Boolean]'
Object.prototype.toString.call(Symbol())        // '[object Symbol]'
Object.prototype.toString.call(null)            // '[object Null]'
Object.prototype.toString.call(undefined)       // '[object Undefined]'

Object.prototype.toString.call(new Date())      // '[object Date]'
Object.prototype.toString.call(Math)            // '[object Math]'
Object.prototype.toString.call(new Set())       // '[object Set]'
Object.prototype.toString.call(new WeakSet())   // '[object WeakSet]'
Object.prototype.toString.call(new Map())       // '[object Map]'
Object.prototype.toString.call(new WeakMap())   // '[object WeakMap]'
```

这是因为：
- 依托Object.prototype.toString()方法得到对象内部属性 `[[Class]]`
- 传入原始类型却能够判定出结果是因为对值进行了包装
- null 和 undefined 能够输出结果是内部实现有做处理

需要注意直接使用对象的`toString()`方法跟用`Object.prototype.toString.call`是不一样的

```javascript
console.log([1,[2,3]].toString())    // 1,2,3
Object.prototype.toString.call([1,[2,3]])     // '[object Array]'
```

### JSON.stringify(obj) ＋ JSON.parse(str) 实现深克隆
用`JSON.stringify(obj)`也可以实现对象深克隆，但是**正则、函数、日期格式数据**会被变成空对象，出现问题。

```javascript
JSON.stringify({a: function() {}, x: 1000})

// "{"x": 1000}"
```

注意：`JSON.stringify(obj)`是把对象转换成字符串

### instanceof 类型检测原理
用`instanceof`作为类型检测的原理是：通过检测**构造函数**的`prototype`属性是否出现在某个实例对象的原型链上。

```javascript
left.__proto__.__proto__... =?= right.prototype
```