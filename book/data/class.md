## Class
es6 提供了面向对象的语法糖，在es5的实现中，每个对象都有 __proto__属性，指向对应构造函数的prototype属性，class同时有prototype属性和 __proto__ 属性，所以存在两条继承链。
- 子类的 __proto__ 属性，表示构造函数继承，总是指向父类对象（在es5中子类作为构造函数__proto__本来应该指向Function.prototype）
- 子类prototype属性的 __proto__ 属性，表示方法的继承，总是指向父类的 prototype 属性（本来指向object.prototype)

类继承的实现:
```javascript
class A {
}

class B {
}

// B的实例继承A的实例
Object.setPrototypeOf(B.prototype, A.prototype);

// B继承A的静态属性
Object.setPrototypeOf(B, A);
```
Object.setPrototypeOf 的实现：
```javascript
Object.setPrototypeOf = function (obj, proto) {
  obj.__proto__ = proto;
  return obj;
}
```
作为一个对象，子类B的原型（__proto__属性）是父类A；作为构造函数，子类B的原型（prototype属性）是父类的实例。

### class不足
1. 不支持静态属性（除函数）

[Javascript的继承与多态](https://www.jianshu.com/p/5cb692658704)
[es6之class原理分析](https://blog.csdn.net/qq_41694291/article/details/103943481)
[理解 es6 中class构造以及继承的底层实现原理](https://www.cnblogs.com/memphis-f/p/12029574.html)
