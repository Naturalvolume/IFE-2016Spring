# 手动实现new创建对象
首先，我们知道new构造函数的时候发生了以下过程：
- 创建一个空对象`{}`
- 在空对象中添加属性`__proto__`指向构造函数的原型
- 将构造函数中`this`绑定的属性添加到空对象中
- 返回`this`

了解了new的过程我们就能手动实现 new 的功能了
### 简单版本new
```javascript
function newNew(n, a) {
  let _this = {
    __proto__: newNew.prototype,
    name: n,
    age: a
  }
  
  return _this
}
let person = newNew('kathy', 18)
console.log(person)   // {age: 18, name: "kathy", __proto__: Object}
```
### 复杂版本new
用 apply 实现 this 指向的变化
```javascript
function A(name, age) {
  this.name = name
  this.age = age
}

function New(func) {
    // step 1: 创建空对象
    var res = {};
    // step 2:添加原型
    if (func.prototype !== null) {
        res.__proto__ = func.prototype;
    }
    // step 3:改变this指向
    // 这里用 apply，传入参数数组是截取 arguments 的
    var ret = func.apply(res, Array.prototype.slice.call(arguments, 1));
    // step 4:返回对象
    // 要先判断 ret 是否为对象，是对象了才能返回
    if ((typeof ret === "object" || typeof ret === "function") && ret !== null) {
        return ret;
    }
    return res;
}
var obj = New(A, 'kathy', 18);
console.log(obj);  // {age: 18, name: "kathy", __proto__: Object}
// equals to
//var obj = new A(1, 2);
```