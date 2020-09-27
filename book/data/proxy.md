# Proxy
Proxy用于修改某些操作的默认行为，可以对外界的访问进行过滤和改写，用于在语言层面做出修改，属于一种“元编程”。
### Proxy实例的构造
```javascript
var proxy = new Proxy(target, handler)
// target: 要拦截的目标对象
// handler: 也是对象，用来定制拦截行为
```

### Proxy支持的拦截操作
- get(target, propKey, receiver): 拦截对象属性的读取，如`proxy.foo`和`proxy[foo]`会被拦截
- set(target, propKey, receiver): 拦截对象属性的设置，如`proxy.foo = v`和`proxy[foo] = v`，返回布尔值
- has(target, propKey): 拦截`propKey in proxy`的操作，返回一个布尔值
- deleteProperty(target, propKey): 拦截`delete proxy[propKey]`的操作，返回布尔值
- ownKeys(target): 拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回target所有的属性，而`Object.keys()`仅返回目标对象自身的可遍历属性
- getOwnPropertyDescriptor(target, propKey): 拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象
- defineProperty(target, propKey, propDesc): 拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。
- preventExtensions(target): 拦截`Object.preventExtensions(proxy)`，返回一个布尔值
- getPrototypeOf(target): 拦截`Object.getPrototypeOf(proxy)`，返回一个对象
- isExtensible(target): 拦截`Object.isExtensible(proxy)`，返回不二之
- setPrototypeOf(target, proto): 拦截`Object.setPrototypeOf(proxy, proto)`，返回布尔值
- apply(target, object, args): 拦截Proxy实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`
- construct(target, args)：拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)`

```javascript
obj = new Proxy(obj, { 
  // target表示监听的对象
  // prop表示改变的属性
  get(target, prop) {
    return target[prop]
  }, 
  // value表示要设置的数据
  set(target, prop, value) {
    
  }
})
```

`Proxy`比`Object.defineProperty`的优势：
- 监听整个对象，而不是对象中的属性
- 新增加的属性也可以监听到
- 不需要克隆数据，通过`target[prop]`可以直接取到数据，没有`Object.defineProperty`的堆栈溢出问题



### this 问题
proxy代理时，对象内部的this关键字会指向Proxy代理
```javascript
const target = {
  m: function () {
    console.log(this === proxy);
  }
};
const handler = {};

const proxy = new Proxy(target, handler);

target.m() // false
proxy.m()  // true
```
# Reflect
与Proxy对象一样，也是es6为了操作对象而提供的新api，设计目的有以下几个：
- 将Object对象的一些明显属于语言内部的方法 转移到 Reflect 上
- 修改某些Object方法的返回结果，让其变得更合理
- 让Object操作都变成函数行为，如`name in obj`变成`Reflect.has(obj, name)`
- Reflect对象和Proxy对象的方法一一对应，目的是让Proxy对象可以方便的调用对象的Reflect方法，完成默认行为，作为修改行为的基础

```javascript
Proxy(target, {
  set: function(target, name, value, receiver) {
    var success = Reflect.set(target, name, value, receiver);
    if (success) {
      console.log('property ' + name + ' on ' + target + ' set to ' + value);
    }
    return success;
  }
});
```