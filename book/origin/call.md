# 实现call apply bind改变this指向
### call
复杂的实现方式
```javascript
  Function.prototype._call = function (context) {
    context = context || window;
    context.fn = this;

    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }

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
```javascript
```
```javascript
```