## 作用域
```javascript
// 这里必须是 let 类型，bar()调用时才能取到a
var a = 5;
var obj = {
  a : 10,
  foo: function(){
    console.log(this.a)
  }
}

var bar = obj.foo
obj.foo()     // 10
bar()       // 5
```
### 箭头函数作用域
# 箭头函数
关于箭头函数的作用域是经常会弄错的一个知识点，箭头函数自己没有作用域，而是向上层查找作用域。
```javascript
// 不使用箭头函数
let a = 'kaola'

let obj = {
    a: '程序员成长指北',
    foo: function() {
        console.log(this.a)
    }
}

obj.foo()             // 输出结果: '程序员成长指北'

// 使用箭头函数
let a = 'kaola'

let obj = {
    a: '程序员成长指北',
    foo: () => {
        console.log(this.a)
    }
}

obj.foo()             // 输出结果: "koala"
```