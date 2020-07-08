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
commal('ab-gi-du')
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
//console.log(brackets('<abbbbb>'))
// 7.分割数字每三个以一个逗号划分
function slicestr(str) {
  let reg = /\d{3}/
  // 这里等号是啥意思
  //let reg = /(\d)(?=(\d{3})+$)/g;
  console.log(reg.exec(str))
 
  // 不能用split
  // $1指代匹配到的每个子字符串，为啥自己写的那个不能用呢
  //return str.replace(reg, '$1,')
  return str.replace(reg, function(word) {
    return word + ','
  })
}
// 一堆问题：exec为啥返回的不是所有的
console.log(slicestr('12345678998'))
```