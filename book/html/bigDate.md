# 在前端如何处理大量数据，让页面不卡顿？
从两方面优化，减少主线程阻塞时间：降低重绘次数 和 缩短循环时间

### DocumentFragment
DocumentFragment 是没有父级文件的最小文档对象，相当于轻量级的document，可用 append 或 insert 等方式存储其他节点，然后一次性加入到 dom 树中，减少重绘次数。DocumentFragment具有以下特性：
- 不属于 dom 节点
- 它的变化不会引起dom树的变化，也就不会重绘
[documentFragment和template](https://www.jianshu.com/p/ebe8e26076dc)
### requestAnimationFrame
[requestAnimationFrame详解](https://www.jianshu.com/p/fa5512dfb4f5)
### 事件委托
### 参考
[如何解决页面加载海量数据而不冻结前端UI](https://www.askhtml5.com/question/3)
