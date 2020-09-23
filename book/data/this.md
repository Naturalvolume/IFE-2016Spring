# this指向
### 一、es5中三种函数调用形式
```javascript
func(p1, p2) 
obj.child.method(p1, p2)
func.call(context, p1, p2)
```
这三种形式中前两种其实是第三种的语法糖，第三种形式才是正常的调用形式。所以`func(p1, p2)`在严格模式下等价于`func.call(undefined, p1, p2)`，在非严格模式下等价于`func.call(window, p1, p2)`，`obj.child.method(p1, p2)`等价于`obj.child.method.call(obj.child, p1, p2)`。

- 函数被独立调用
在严格模式下，this 指向 undefined，非严格模式下 this 指向 window。

```javascript
// 严格模式下
"use strict"
var a = 10;
function foo() {
  var a = 20;
	console.log(this.a); // this -> undefined
}
foo();      // Uncaught TypeError: Cannot read property 'a' of undefined

// 非严格模式下
var a = 10;
function foo() {
  var a = 20;
	console.log(this.a); // this -> window
}
foo();    // 10
```

- 函数被 obj 调用

严格模式和非严格模式，this都指向 obj。
```javascript
var a = 10;

var obj = {
  a: 20,
  fn() {
    console.log(this.a);
  }
}

obj.fn(); // 20 this -> obj
```

最后，来看一个综合例子
```javascript
var age = 5;
var person = {
  age : 10,
  getAge: function(){
    console.log(this.age)
  },
  setAge: {
    age: 18,
    getAge: function(){
      console.log(this.age)
    }
  }
}

var bar = person.getAge
// 情况一：函数定义在对象中
person.getAge()   // 等价于 person.getAge.call(person)
            // 输出 10
bar()   // 等价于 bar.call(window)
        // 输出 5
person.setAge.getAge()   // 等价于 person.setAge.getAge.call(person.setAge)
```
### 二、变量的this
- 全局变量的this
全局变量有：`var`定义的变量、未用关键字定义的变量

全局变量在严格模式下无 this 绑定，在非严格模式中被绑定到 window 对象上。

```javascript
// 严格模式
"use strict";
var a = 1
console.log(this.a)   // undefined   this -> window

// 非严格模式
var a = 1
console.log(this.a)   // 1    this -> window
```
- 局部变量的this

局部变量是定义在函数中的变量，在严格和非严格模式下都不会挂载到对象上，所以局部变量无法用 this 取到。

```javascript
var a = 2
function A() {
  var a=1
    
  console.log(a)    // 1
  console.log(this.a)   // 2  this -> window
}
  A()
  console.log(this)   // window
  console.log(this.a)   // 2
```
- let 变量的this

｀let｀变量在严格模式和非严格模式下都不会被挂载到 window 对象上。

```javascript
// a 不会被挂载到window上
let a = 10;
function foo() {
  var a = 20;
  console.log(this.a); // this -> window
                      // undefined
}
console.log(this.a)   // undefined
foo();   
```

- 对象属性中的 this
在严格和非严格模式下，对象属性中的this指向 window，这跟变量的this指向和函数的this指向都是不一样的。

```javascript
// 严格模式
"use strict"
var a = 10;

var obj = {
	a: 20,
	b: this.a,   // this -> window
}
console.log(obj.b)    // 10

// 非严格模式
var a = 10;

var obj = {
	a: 20,
	b: this.a,   // this -> window
}
console.log(obj.b)    // 10
```

### 三、this绑定原则
- 默认绑定

1.函数独立调用时严格模式下 this 绑定为 undefined，非严格模式下绑定为 window
2.`setTimeOut`和`setInterval`中的this指向，在严格和非严格模式下指向的都是全局对象。

```javascript
"use strict" (or none)
var a = 10;

function foo() {
	var a = 20;

	setTimeout(function() {
	  var a = 30;
	  console.log( this.a ); // 10 this -> window
	})
}

foo();
```

- 隐式绑定
1.调用对象函数的时候`obj.child.method(p1, p2)`，this 默认指向函数的上一层对象，即`obj.child`；
2.dom事件中的 this 默认指向 dom 元素对象。
- 显式绑定
1.call
2.apply
3.bind
关于这三个函数的作用，在这里不做过多讲解。
- new 绑定
用构造函数 new 一个实例的时候，会把实例的this自动绑定会实例，关于如何手动实现 new，可以看这里：[手动实现new创建对象]()

上面四种绑定的优先级为：
new绑定 > 显式绑定 > 隐式绑定 > 默认绑定
### 四、箭头函数中的this
首先，我们要清楚一个概念，箭头函数没有自己的arguments、原型、构造函数，它有自己的`this`，箭头函数初始化时所在环境的this就是它所绑定的，所以箭头函数是不能使用`call`、`apply`、`bind`绑定 this 的，但是箭头函数在**运行时**会向上一层查找自己需要的this、arguments等。

```javascript
var a = 10;
var obj = {
	a: 20,
  fn: () => {
    var a = 30;
	  console.log(this.a);    
	}
}

obj.fn();   // 不会调用函数 obj.fn.call(obj)
            // 向 obj 上一层查找 this
            // 找到 this -> window，输出 10
```

```javascript
var a = 10;
var obj = {
	a: 20,
	fn: function() {
	    (() => {
	        var a = 30;
	        console.log( this.a );
	    })();
	}
}

obj.fn();   // fn 不是箭头函数，隐式使用 obj.fn.call(obj)
            // 执行 fn，遇到箭头函数，箭头函数无 this 
            // 使用上一层 obj 的 this，输出 20 
```
有了上面两个的分析，你应该很快就能分析出下面这个例子输出什么了

```javascript
var a = 10;
var obj = {
	a: 20,
	fn: function() {
	    (() => {
	        var a = 30;
	        (() => {
	            var a = 40;
	            (() => {
	                console.log( this.a );
	            })()
	        })();
	    })();
	}
}

obj.fn();     
```

答案是 20！
### 终极考察
```javascript
var length = 10;
function fn() {
    console.log(this.length);
}
 
var obj = {
  length: 5,
  method: function(fn) {
    fn();
    arguments[0]();
  }
};
 
obj.method(fn, 1);//输出是什么？

```

输出 10 2

- 10就是正常的对象上函数调用，但是2是如何来的呢？

这是因为`arguments`是类数组对象，所以它本质上是对象，`arguments[0]()`相当于调用`arguments`对象上的第一个属性，根据函数调用的语法糖`arguments.call(arguments, fn)`，`this`此时指向`arguments`对象，`arguments.length`就是2。