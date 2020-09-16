# 数组和字符串的存储原理
### 数组
通常意义上数组是一顿连续的内存，但在js中数组是哈希映射或字典，可以通过链表等实现，因此js的数组读取操作比传统的数组读取耗时长。

后来改进用连续位置 或 结构化数组ArrayBuffer存储，优化性能。

**参考**：
- [深究 JavaScript 数组 —— 演进&性能](https://juejin.im/entry/59ae664d518825244d207196)
- [ArrayBuffer和Array区别](https://www.jianshu.com/p/5fcbccf1bab1)
- [聊聊JS的二进制家族：Blob、ArrayBuffer和Buffer](https://www.cnblogs.com/penghuwan/p/12053775.html)