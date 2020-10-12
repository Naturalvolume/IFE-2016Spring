# html标签
[HTML常用标签及其属性](https://www.cnblogs.com/dear_diary/p/6510393.html)

### 一些看不见的标签
看不见的标签大多数用在页面头部`head`标签内，虽然对用户不可见，但在某些场景，如交互实现、性能优化、搜索优化等，可以使用它们达到目标。
- 交互实现
  - `meta`：自动刷新／跳转，缺点是不可取消此次跳转或刷新操作

```javascript
// 实现 5s 后跳转到 同域下 的其它页面
<meta http-equiv="Refresh" content="5; URL=page2.html">

// 实现隔一分钟刷新页面
<meta http-equiv="Refresh" content="60">
```

  - `title`标签：消息提醒、文字滚动、提示关键信息（如下载的进度、当前操作步骤等）

```javascript
// html5可使用 Web Notifications API 实现弹出系统消息

// html5 之前通过动态修改title名称实现消息提醒闪烁功能
let msgNum = 1 // 消息条数
let cnt = 0 // 计数器
const inerval = setInterval(() => {
  cnt = (cnt + 1) % 2
  if(msgNum===0) {
    // 通过DOM修改title
    document.title += `聊天页面`
    clearInterval(interval)
    return
  }
  const prefix = cnt % 2 ? `新消息(${msgNum})` : ''
  document.title = `${prefix}聊天页面`
}, 1000)
```

- 性能优化
  - `script`标签：调整加载顺序提升渲染速度

> 在浏览器底层运行机制中，解析html时，遇到script标签会停止解析，通知 **网络线程** 加载文件，文件加载后切换至**js引擎**执行对应代码，代码执行完成，再切换至**渲染引擎**继续渲染页面。

```javascript
// 1. async 属性：立即请求文件，但不阻塞渲染引擎，文件加载完毕后阻塞渲染引擎并立即执行文件内容
<script async></script>
// 2. defer 属性：立即请求文件，不阻塞渲染引擎，等解析完html后再执行文件内容
<script defer></script>
// 3. html5标准type属性，对应值为"module"，让浏览器按标准将文件当作模块进行解析，默认阻塞效果同defer，也可配合async在请求完成后立即执行
<script type="module"></script>
<script type="module" async></script>
```

  - `link`标签：通过预处理提升渲染速度（合理利用`link`标签的`rel`属性进行预加载，提升渲染速度）

```javascript
// 1. dns-prefetch：浏览器先对某个域名进行dns解析并缓存，因此在浏览器请求同域名资源时，能省去从域名查询ip的过程，减少时间损耗
<link rel="dns-prefetch" href="//baidu.com"></link>

// 2. preconnect：浏览器在http请求正式发给服务器前预先执行一些操作，包括dns解析、TLS协商、TCp握手，通过消除往返延迟为用户节省时间
// 3. prefetch/preload：都是让浏览器预先下载并缓存某个资源，但prefetch 可能会在浏览器忙时被忽略，而preload则是一定会被预先下载
// 4. prerender：浏览器不仅会加载资源，还会解析执行页面，进行预渲染
```

- 搜索优化
  - `meta`标签：提取关键信息，搜索引擎展示搜索结果

```javascript
// 1. meta 标签可以设置页面的描述信息
// 2. 还可以使用关键字，这样即使页面其它地方没有包含搜索内容，也可以被搜索到（搜索引擎还有自己的权重和算法，所以若滥用关键字会被降权，如 Google 引擎就会对堆砌大量相同关键词的网页进行惩罚，降低它被搜索到的权重）
<meta content="拉勾微信招聘, 微博招聘, 校招,社会招聘,社招" name="keywords">
```

参考：[拉钩教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=180#/detail/pc?id=3171)

#### 什么是标签语义化
合理的标签干合适的事情，合理的标签有块状标签(div、p、h1~h6、main、footer)、行状标签() 和 行块状标签()。

三类标签的区别：

如何转换：

display的取值：

#### 让div消失的方法（7、8种）
opacity 的兼容处理
filter 还能做哪些事情
