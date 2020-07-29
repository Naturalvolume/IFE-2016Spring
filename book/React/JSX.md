# JSX
### 什么是JSX?
JSX是 javascript 的语法扩展，是React发明的可以很好地描述UI，并且JSX可以生成React元素。JSX是javascript和XML结合的一种格式，当遇到 `<>` 就当作html解析，遇到`{}`当作js解析。
```javascript
const element = <h1>Hello, {name}</h1>;
```

JSX也是表达式，在编译之后会转为普通js函数调用，JSX可以这样使用：
- 可以在JSX中嵌入任何有效的javascript表达式，如2+2、user.firstName、调用函数等
- 可以在 if 语句和 for 循环的代码块中使用JSX
- 将JSX赋值给变量
- JSX当作参数传入
- 从函数中返回JSX
### JSX 防止注入攻击
可以在JSX当中插入用户输入内容
```javascript
const title = response.potentiallyMaliciousInput;
// 直接使用是安全的：
const element = <h1>{title}</h1>;
```
因为在渲染输入内容之前，默认会进行转义，将所有内容都转换成字符串，可以确保不会注入那些并非自己明确编写的内容，防止XSS（跨站脚本攻击）。
### babel转译
babel 会把 JSX转译成一个名为React.createElement()函数调用。
```javascript
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```
转译成
```javascript
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```