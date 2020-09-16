# 在前端如何处理大量数据，让页面不卡顿？
减少主线程阻塞时间，从两方面优化：降低重绘次数 和 缩短循环时间

### document.createDocumentFragment()——批处理，减少回流次数
DocumentFragment 是没有父级文件的最小文档对象，相当于轻量级的document，可用 append 或 insert 等方式存储其他节点，然后一次性加入到 dom 树中，减少dom操作次数，降低回流对性能的影响。

DocumentFragment具有以下特性：
- 不属于 dom 节点
- 它的变化不会引起dom树的变化，也就不会重绘
- 可以作为append类（`Node.appendChild`、`Node.insertBefore`）方法多参数，但是只有它的children被append，本身却不会。

[documentFragment和template](https://www.jianshu.com/p/ebe8e26076dc)
### requestAnimationFrame——在页面重绘前插入新节点
[requestAnimationFrame详解](https://www.jianshu.com/p/fa5512dfb4f5)
### 事件委托
利用js的事件冒泡机制，实现对海量元素对监听，有效减少事件注册数量。
### 参考
[如何解决页面加载海量数据而不冻结前端UI](https://www.askhtml5.com/question/3)
