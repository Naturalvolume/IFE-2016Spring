## javascript 中的作用域
在js中，生成作用域的方式：
- 函数：在大多数语言中都是块作用域，以 `{}`限定，js里面叫函数作用域，就是一个变量在全函数里有效，比如有个变量p1在函数最后一行定义，第一行也有效，但是值是undefined。所以es6中引入的 let 变量是块级作用域，意思就是变量作用域在 `{}`中有效，且不能提升。
- with、eval（不建议使用，影响性能）

语言中常见的作用域有两种，词法作用域与动态作用域，在js中用的是词法作用域。
### 词法作用域／静态作用域
首先，我们要知道编译器的第一个工作阶段是分词，就是把由字符组成的字符串分解成词法单元，所以**词法作用域**就是定义在词法阶段的作用域，是由写代码时将变量和块作用域写在哪里决定的，因此**词法分析器处理代码时会保持作用域不变**。

个人理解，在js中词法作用域就是不受 this 的影响，函数所用变量由它定义在哪儿决定。
### 动态作用域
动态作用域不关心函数和作用域是如何声明以及在何处声明的，只关心它们从何处调用，即动态作用域链是**基于调用栈**的，而不是代码中的作用域嵌套。

个人理解，在js中动态作用域就是由 this 影响的作用域，比如构造函数 new 的时候会隐式返回 this 对象，这个this对象会被加入到作用域链中，非构造函数就没有这个 this 作用域。
```javascript
var age = 19
function Person() {
  this.age = 10
  this.hisAge = () => {
    console.log(this.age)
  }
}
let p = new Person()

console.log(p)  // 其中 hisAge 的作用域链为 [[Scopes]]: Scopes[2]
                //  0: Script {p: Person}
                // 1: Global {parent: Window, opener: null, top: Window, length: 0, frames: Window, …}

p.hisAge()  // 10
``` 
### 词法作用域和动态作用域的区别
首先，看一个经典例子
```javascript
var a = 2;
function foo() {
    console.log( a );
}
function bar() {
    var a = 3;
    foo();
}
bar();
```
若是词法作用域，上例输出 2，若是动态作用域，上例输出 3。
在控制台中输出 2，所以javascript是**词法作用域**。
### 函数作用域
在js中绝大多数作用域都是基于函数生成的

### 重点：函数提升 ＋ 变量提升
- 函数声明优先级最高，提升后会覆盖同名变量；
- 函数表达式 只有变量提升，函数声明不会被提升；
- 在赋值时，后面的同名变量总会覆盖前面的。
```javascript
// 1. 变量声明，提升，a 为 undefined
// 2. 函数声明，提升，a 为 function a() {}
console.log(a)    // function a(){}
function a(){}
// 3. 变量赋值， a 为 2
var a = 2
console.log(a)    // 2
```
```javascript
// 1. 遇到第一个 a 变量声明，提升，a 为 undefined
// 2. 遇到第二个 a，不会提升了
console.log(a)    // undefined
// 3. 变量赋值， a 为 function a() {}
var a = function a(){}
console.log(a)    // function a() {}
// 4. 变量赋值， a 为 2
var a = 2
console.log(a)    // 2
```