## 关于virtual dom的几个误区
最终我们识别了几个关于 Virtual DOM 优势误区：

- 操作 DOM 太慢，操作 Virtual DOM 对象快 ❌

Virtual DOM 很快，但这并不是它的优势，因你本可以选择不使用 Virtual DOM 。

- 使用 Virtual DOM 可以避免频繁操作 DOM ，能有效减少回流和重绘次数 ❌

无论你在一次事件循环中调用多少次的 DOM API ，浏览器也只会触发一次回流与重绘（如果需要），并且如果多次调用并没有修改 DOM 状态，那么回流与重绘一次都不会发生。批量操作也不能减少回流与重绘。

- Virtual DOM 有跨平台优势 ❌

跨平台是 Javascript 的优势，与 Virtual DOM 无关。

#### 我们也提到了 Virtual DOM 真正的优点是其抽象能力和常驻内存的特性，让框架能更容易实现更强大的 diff 算法，缺点是增加了框架复杂度，也占用了更多的内存。