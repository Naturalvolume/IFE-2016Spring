# 装饰器
装饰器是一种与类相关对语法，用来注释或修改类和类方法，类似于设计模式中的装饰器模式，作用是**可以实现代码的复用**。

### 类的装饰
1. 为类添加静态属性／方法

```javascript
// 定义装饰器函数，target即要改变行为的类
function testable(target) {
  target.isTestable = true
}
// 用装饰器函数装饰类
@testable class MyTestableClass {

}
// 这样 装饰器类就有了 isTestable 方法
// MyTestableClass.isTestable    =>   true

@testable class Person {

}
// Person类也有 isTestable 方法了
```

装饰器对类对行为的改变 是**代码编译时发生的**，而不是在运行时，所以装饰器本质就是编译时执行的函数，且`new`之前**类已经被改变了**。

2. 给类添加高阶函数

```javascript
function testable(isTestable) {
  return function(target, ...rest) {
    console.log('参数', rest)
    target.isTestable = isTestable
  }
}

// 相当于先执行 testable(true)，返回的函数才是会装饰类的函数
@testable(true)
class MyTestableClass {}
```

3. 给类的原型方法添加属性

**class类**
```javascript
// target代表class类，name是属性的名称，descriptor相当于描述对象上属性特征的对象
function readonly(target, name, descriptor) {
  console.log('target', target)
  console.log('name', name)
  console.log('descriptor', descriptor)
  // descriptor对象的值
  /*
  {
    value: specifiedFunction, 值,
    enumerable: false,
    configurable: true,
    writable: true
  }
  */
}

class Person {
  @readonly
  abc() {}

  @readonly
  x = 123
}
```

**function类**：通过目标类的`prototype`对象操作

```javascript
function testable(target) {
  target.prototype.isTestable = true;
}

@testable
class MyTestableClass {}

let obj = new MyTestableClass();
obj.isTestable // true
```

