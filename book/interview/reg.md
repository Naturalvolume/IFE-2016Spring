# 正则表达式
### 元字符的两种定义方式
元字符在正则表达式中有特殊含义，如 /d 表示数字类字符，定义含有元字符的正则表达式是这样的：
- 字面量表达式直接定义：`let reg = /\d/`  
- 构造函数式要加转义字符：`let reg = new RegExp("\\d")`

### global修饰符
**global/g**：全文搜索，和 lastIndex 属性配合，每次保存上一次匹配结束的位置，下一次继续在该位置搜索。

只在正则对象的方法中起作用，如`RegExp.prototype.test(str)`和`RegExp.prototype.exec(str)`，对字符串的方法不起作用。
### RegExp.prototype.test(str) 和 String.prototype.search(reg)
这两个方法都可以用来查找字符串 str 中是否有对应的正则表达式字符串，但有以下不同：
- test 是正则对象的方法，search 是字符串对象的方法
- test 在全局模式下与 RegExp.lastIndex 配合使用，每次都从上一次匹配的位置出发
- test 返回布尔值，只判断是否存在匹配，而 search 返回第一个匹配到的位置，没有则返回 -1

```javascript
let str = "k is so k"
// 全局模式
let reg = new RegExp("k", "g")
console.log(reg.test(str))    //true
console.log(reg.lastIndex)    // 1
console.log(reg.test(str))    //true
console.log(reg.lastIndex)    // 9
```

```javascript
// 全局模式
let str = "k is so k"
let reg = new RegExp("k", "g")
console.log(str.search(reg))  // 0
console.log(reg.lastIndex)    //0
console.log(str.search(reg))  //0
console.log(reg.lastIndex)    // 0
```

### RegExp.prototype.exec(str) 和 String.prototype.match(reg)
exec 和 match 方法都可以返回有 索引位置index 属性的对象，但两者的具体使用与是否在全局模式下有关系。

- exec 在非全局模式下每次返回第一个匹配对象，在全局模式下和 lastIndex 属性配合使用，每次都向后查找一个，返回对应的匹配对象。

```javascript
let str = "k is so k"
// 非全局模式
let reg = new RegExp("k")
console.log(reg.exec(str))    // {"0: "k"
                              // groups: undefined
                              // index: 0
                              // input: "k is so k k k"
                              // length: 1"}
console.log(reg.lastIndex)    //  0
console.log(reg.exec(str))    // 同上
console.log(reg.lastIndex)     // 0
// 全局模式
let str = "k is so k"

// 非全局模式
let reg = new RegExp("k", "g")
console.log(reg.exec(str))    // {"0: "k"
                              // groups: undefined
                              // index: 0
                              // input: "k is so k"
                              // length: 1"}
console.log(reg.lastIndex)    //  0
console.log(reg.exec(str))    // {"0: "k"
                              // groups: undefined
                              // index: 8
                              // input: "k is so k"
                              // length: 1"}
console.log(reg.lastIndex)     // 9
```

- match 在全局模式下直接返回 匹配对象数组，没有位置 index 等信息，在非全局模式下返回第一个对象的匹配对象，包含 index 等信息。

```javascript
// 全局模式
let str = "k is so k"
let reg = new RegExp("k", "g")
console.log(str.match(reg))   // ["k", "k"]
console.log(reg.lastIndex)    // 0
console.log(str.match(reg))   // ["k", "k"]
console.log(reg.lastIndex)    // 0

// 非全局模式
let str = "k is so k"
let reg = new RegExp("k", "g")
console.log(str.match(reg))   // {"0: "k"
                              // groups: undefined
                              // index: 0
                              // input: "k is so k"
                              // length: 1"}
console.log(reg.lastIndex)    // 0
console.log(str.match(reg))   // 同上
console.log(reg.lastIndex)    // 0
```

### String.prototype.replace(reg, replacement)
replace 方法用 replacement 替换对应的匹配，返回替换后的字符串。

replacement 可以是
- 字符串
- 函数，函数的参数是匹配到的字符串
但是不能是箭头函数，因为箭头函数不能取到匹配到的字符串。

下面是几个 replace 的例子：
```javascript
// 1.get-element变成驼峰式
function commal(str) {
  // 不规定个数都是自动匹配一个
  // 一定要加全局修饰符
  let reg = new RegExp('-[a-z]', 'g')
  // $0 是replace匹配到的字符串
  // replace方法不改变原有字符串
  return str.replace(reg, function($0) {
    return $0.slice(1).toUpperCase()
  })
}

console.log(commal('ab-gi-du'))    // abGiDu
```

```javascript
// 2.分割数字每三个以一个逗号划分
function slicestr(str) {
  let reg = /\d{3}/
  // 这个是正确的，不会出现 123,256, 的情况
  //let reg = /(\d)(?=(\d{3})+$)/g;
 
  // 使用箭头函数，取不到 匹配到的字符串，最后返回 undefined
  /* return str.replace(reg, (word) => {
    word + ','
  }) */
  return str.replace(reg, function(word) {
    return word + ','
  })
}
```

### $ 的用法
$0,$1....$9 是表示正则匹配的组

### 正则表达式的使用
```javascript
// 1. 在键值对字符串中，匹配到 -webkit 返回浏览器内核
// "key1 = value, key2 = value, -webkit = google, key3 = value, -webkit = google, -webkit = google"
function keyValue(str) {
  let reg = new RegExp('-webkit')
  let index = str.match(reg).index
  // 截取对应字符
  return str.substr(index, 16)
}

// 2 . 匹配二进制数字
function two(str) {
  // 字面量表达式
  // [01] 表示字符集，泛指，只有要1或0
  // ^ &表示要以 1或者无数个[01]字符集 开始且结尾
  var reg = /^[01]+$/;
  return reg.test(str);
}
two('101109')  // false
// 3. 非零的十进制数字 (有至少一位数字, 但是不能以0开头)
function tennum(str) {
  var reg = /^[1-9][0-9]?$/g;
  return reg.test(str)
}
// 4 . 匹配一年中的12个月
function month(str) {
  // | 代表或
  // 加不加括号有什么影响呢
  let reg = new RegExp('^0?[1-9]|1[0-2]$')
  return reg.test(str)
}
//console.log(month('12'))
// 5 . 匹配qq号最长为13位
function qq(str) {
  // 为啥 /d 不管用
  //let reg = new RegExp('^[1-9][0-9]{9,12}$')
  // /d 在字面量中就可以了！！！
  let reg = /^[1-9]\d{9,12}$/
  return reg.test(str)
}
// 6.匹配用尖括号括起来的以a开头的字符串  "<a herf='www.baidu.com'>";
function brackets(str) {
  let reg = /<a[^>]+>/g
  return reg.exec(str)
}
```