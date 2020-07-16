## Doctype
`Doctype`位于html文档的第一行，告知浏览器解析器用什么文档标准解析文档。

解析文档的标准有：
- 标准模式／严格模式：以该浏览器支持的最高标准运行，比如 **盒模型** 以标准模式显示（不管是ie浏览器还是其他的浏览器）。
- 兼容模式／怪异模式：以向后兼容的方式显示，模拟老式浏览器的行为，比如**盒模型** 在该模式下，会在不同的浏览器中显示不同的形式，比如在ie浏览器中以ie盒模型显示。

### 结论
- Doctype 在一定程度上会影响盒模型，但不能准确定义使用的盒模型，最好不要使用Doctype定义。
- box-sizing 是切换盒模型的最佳形式。
- 在日常使用中，文档头部加`<!Doctype html>`.
- 标准盒模型是定义元素的最佳模式，但在一些条件下，还是需要用 box-sizing：border-box； 切换成ie盒模型，让布局更加美观。
- box-sizing 是不可继承属性。

**问题**：
- 什么情况下最好使用 box-sizing：border-box； 切换成ie盒模型？
- box-sizing 是不可继承属性是什么意思？

### 参考
[CSS中的盒子模型与 box-sizing 属性](https://www.pianshen.com/article/9833528158/)
[前端面试题及其总结](https://juejin.im/post/5c5ab7dae51d4501333fc60f)
[每日思考: DOCTYPE的作用是？](https://juejin.im/post/5c35bc1c6fb9a049c64408bf)