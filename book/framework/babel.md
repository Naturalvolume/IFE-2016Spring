# babel
babel 是一个**js的编译器**，可以把最新版本的js编译成低版本的，所以通过babel我们可以使用高版本的js语法。
### 配置文件 .babelrc
babel 的配置文件是 .babelrc，存放在项目根目录下，使用babel第一步，就是配置这个文件 设置转码规则和插件：
```json
{
  // presets 设置转码规则
  "presets": [
    "@babel/env",
    "@babel/preset-react",
    // 用于命令行转码
    "@babel/cli",
    // 提供支持es6 的node环境
    "@babel/node",
    // 改写require命令，以后每当用require加载.js .jsx .es .es6后缀名的文件
    // 会先用babel转码
    // 只是应用require命令加载的文件转码，不会对当前文件转码
    // 且它是实时转码，只适合在开发环境使用
    "@babel/node"
  ],
  "plugins": []
}
```
- polyfill：babel默认只转换新的js句法，而不转换新的api，比如 Iterator、Generator、Set、Map、Proxy、Reflect、Symbol、Promise 等全局对象，以及一些定义在全局对象上的方法（比如 Object.assign）都不会转码。


### babel运行原理
#### 第一阶段：解析
接收代码并输出AST，分为两个阶段：词法分析 和 语法分析
- 词法分析：把字符串形式代码转换成tokens流
- 语法分析：使用token中的信息把一个令牌token转换成AST形式，易于后续操作

babel 使用 **@babel/parser** 解析代码，输入的js代码字符串根据ESTree规范生成AST（抽象语法树）。

#### 第二阶段：转换
转换步骤接收AST并对其进行遍历，对节点进行添加、更新及移除等操作，是babel或其它编译器中最复杂的过程。

babel 使用 **@babel/traverse** 方法维护AST树的整体状态，完成对树的替换、删除或者增加节点，这个方法参数为原始AST和自定义的转换规则，返回结果为转换后的AST。

#### 第三阶段：生成
代码生成阶段把最终经过一系列转换之后的AST转换成字符串形式的代码，同时还会创建**源码映射**。

babel 使用 @babel/generator 将修改后的AST转换成代码，生成过程可以对**是否压缩以及是否删除注释等进行配置**，并支持**sourceMap**。

### babel包
- babel-core：babel的核心包，里面存放着很多核心api

### 撸一个babel插件
简单的es6转es3：
- let const -> var
- 箭头函数 -> 普通函数


参考：[深入浅出 Babel 上篇：架构和原理 + 实战](https://juejin.im/post/6844903956905197576#heading-0)