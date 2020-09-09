# 前端异常处理方案
### 需要解决的异常有
- js语法错误、代码异常
- ajax请求异常
- 静态资源加载异常
- promise异常
- iframe异常
- 跨域script error
- 崩溃和卡顿

### 1. try-catch捕捉异常
只能捕获到同步运行时的错误，语法和异步错误捕获不到。
通常用来在可预见情况下监控特定的错误
### 2. window.onerror()
js运行时错误会触发window.onerror，通常用来捕获预料之外的错误
### 3. window.addEventListener
资源（如图片或脚本）加载失败，失败会被window.addEventListener捕获
### 4. Promise Catch
在promise中使用catch可以非常方便的捕获到异步 error，但是没有写 catch 的 Promise 中抛出的错误无法被 onerror 或 try-catch 捕获到


参考：[如何优雅处理前端异常？](https://mp.weixin.qq.com/s/prf-mXexBh1Ie-ctq9FnzA)