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
### 5. 基于service Worker的崩溃统计方案
- Service Worker 有自己独立的工作线程，与网页区分开，网页崩溃，Service Worker一般不会崩溃
- Service Worker 生命周期一般比网页要长，可以用来监控网页的状态
- 网页可以通过`navigator.serviceWorker.controller.postMessage`向掌管自己的SW发送消息

实现一种基于心跳检测的监控方案：
- 网页加载后，通过`postMessage`每5s给 sw 发送一个心跳，表示自己在线，sw将在线的网页登记下来，更新登记时间
- 网页在beforeUnLoad时，通过postMessage告知自己已正常关闭，sw将登记的网页清除
- 如果网页在运行的过程中crash了，sw中的running状态将不会被清除，更新时间停留在崩溃前的最后一次心跳
- sw: Service Worker 每10s查看一遍登记中的网页，发现登记时间已经超出了一定时间（比如15s）即可判定该网页crash了

参考：[如何优雅处理前端异常？](https://mp.weixin.qq.com/s/prf-mXexBh1Ie-ctq9FnzA)