# 用对象模拟数组
下列代码会输出什么？
```javascript
var obj = {
    '2':3,
    '3':4,
    'length':2,
    'splice':Array.prototype.splice,
    'push':Array.prototype.push
}
obj.push(1)
obj.push(2)
obj.push(3)
console.log(obj)
```

输出
```javascript
{
    '2':1
    '3':2,
    '4':3,
    'length':5,
    'splice':Array.prototype.splice,
    'push':Array.prototype.push
}
```

- 给 obj 加上长度，相当于是类数组
- 调用数组上的push方法，会在数组的最后加一项，第一次调用，长度变为3，所以下标为2的那一项会被赋值为1
- 数字2作为对象的key值时，会被转为字符串，所以会覆盖掉本来的key为`'2'`