# 实现call apply bind改变this指向
### call
复杂的实现方式
```javascript
  Function.prototype._call = function (context) {
    context = context || window;
    context.fn = this;

    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
      // 传递字符串参数时，在这里会先解析字符串参数，
      // 然后在 eval 中，" " + args + " " 这样的字符串拼接，会让原来的字符串参数变成普通参数
      // 所以 eval执行时，就不是字符串参数了，会报错
      // 因此把 arguments 解析的过程放在eval中
        args.push('arguments[' + i + ']');
    }
    // 数组转换成字符串，会变成 1,2,3形式
    // context.fn(arguments[1], arguments[2], auguments[3])
    eval('context.fn(' + args +')');

    delete context.fn
}
```
es6的实现方式，用展开字符接收 不是数组的 参数
```javascript
Function.prototype.call = function (context, ...args) {
  var context = context || window;
  context.fn = this;
  // console.log(args)    输出 1,2,3
  var result = eval('context.fn(...args)');

  delete context.fn
  return result;
}
```
### apply 实现
```javascript
// arr 接受传入的参数数组
Function.prototype.apply_ = function (obj, arr) {
    obj = obj ? Object(obj) : window;
    obj.fn = this;
    // 没有参数数组，直接执行
    if (!arr) {
        obj.fn();
    // 有参数，用数组存储
    } else {
        var args = [];
        // 注意这里的i从0开始
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push("arr[" + i + "]");
        };
        eval("obj.fn(" + args + ")"); // 执行fn
    };
    delete obj.fn; //删除fn
};
fn.apply_(obj, ["我的", "名字", "是"]); // 我的名字是听风是风
fn.apply_(null, ["我的", "名字", "是"]); // 我的名字是时间跳跃
fn.apply_(undefined, ["我的", "名字", "是"]); // 我的名字是时间跳跃
```
es6实现，注意与call的不同
```javascript
Function.prototype.call = function (context, args) {
  var context = context || window;
  context.fn = this;

  var result = eval('context.fn(...args)');

  delete context.fn
  return result;
}
```
### bind 实现
bind改变this指向是隐式调用 call 方法
```javascript
Function.prototype._bind = function(context, ...args) {
  // 异常处理，判断要改变指向的是不是函数
  if(typeof this != 'function') throw new Error('what is trying to be bound is not callable')
  // 保存调用的函数
  var fn = this

  let fBound = function() {
    // new 的时候隐式创建 this，指向实例
    // 判断 this 是否是由new创建的，是的话就让函数的this变为实例的this
    return fn.apply(this instanceof fBound ? this : context, args.concat(Array.prototype.slice.call(arguments)))
  }
  // 用空对象当作 fBound 继承 fn 的桥梁
  let fNOP = function() {}
  if(fn.prototype) {
    fNOP.prototype = fn.prototype
  }
  // 这样防止修改 fBound 原型的时候，也改变 fn 的原型
  fBound.prototype = new fNOP()
  return fBound
}

// 调用
function run(a, b) {
  this.val = '1'
  console.log(this.name + a + 'running'+b)
}

let obj = {
  name: 'kathy'
}
let Irun = run._bind(obj, 'a')

let instance = new Irun('b')
console.log(instance.val)
```
参考：[前端面试题——自己实现bind](https://zhuanlan.zhihu.com/p/85438296)
