# 装饰器
装饰器是一种与类相关对语法，用来注释或修改类和类方法，作用类似于设计模式中的装饰器模式。

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
```

装饰器对类对行为的改变 是**代码编译时发生的**，而不是在运行时，所以装饰器本质就是编译时执行的函数。

2. 为类添加实例属性，通过目标类的`prototype`对象操作

```javascript
function testable(target) {
  target.prototype.isTestable = true;
}

@testable
class MyTestableClass {}

let obj = new MyTestableClass();
obj.isTestable // true
```

