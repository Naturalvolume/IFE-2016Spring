### Object.defineProperty
[深入浅出Object.defineProperty()](https://www.jianshu.com/p/8fe1382ba135)

[mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
### obj.propertyIsEnumerable(props)
用来判断属性 props 是否是可以被 for...in 枚举
### Object.assign()
用于将所有可枚举属性的值从一个或多个源对象复制到目标对象，返回目标对象。
```javascript
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
``` 
