# let
- 是es6的语法
- 暂时性死区：不能变量提升

例子一：虽然有全局变量tmp，但是在块级作用域中let又声明了一个局部变量tmp，所以let绑定块级作用域，在let声明前调用tmp，会报错
```javascript
var tmp = 123;

if (true) {
   tmp = 'abc'; // ReferenceError
   let tmp;
}
```

例子二：变量没被声明时，使用typeof不会报错
```javascript
typeof undeclared_variable // "undefined"
```

例子三：行参也存在暂时性死区
```javascript
function bar(x = y, y = 2) {
  return [x, y];
}
bar(); // error
```

es6引入了暂时性死区，让let、const变量不再提升，是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为，这种错误在es5很常见。

暂时性死区的本质就是，**只要一进入当前作用域，所使用的变量就已经存在了，但是不可获取**，只有到声明变量时，才可以获取到变量

- 不能重复声明
- 块级作用域
