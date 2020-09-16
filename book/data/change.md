# js中的进制转换
### 十进制转其它进制，数字转字符
用 toString(n) 方法可以实现十进制转n进制，并且会把数字转成字符串。

```javascript
let num = 234
console.log(num.toString(2))  
console.log(num.toString(8))  
console.log(num.toString(16))  
```

### 其它进制转十进制，字符转数字
用 parseInt(str, n) 可以将 n 进制的 str 转成十进制
```javascript
let str = "101010"
console.log(parseInt(str, 2))   // 42
console.log(parseInt(str, 8))   // 33288
console.log(parseInt(str, 16))  // 1052688
console.log(parseInt(str, 10))  // 101010
```
### 不用语法糖——十进制转八进制
- step1: 十进制用循环求余的方法转成二进制
- step2: 把这些二进制以每三位为一段转成 十进制， 这些十进制就是对应的八进制了