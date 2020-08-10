# 路由
### 服务器端路由（服务器端渲染）
路由概念起源于服务器，服务器根据http请求的url找到对应的资源：
- 静态资源就是文件读取操作
- 动态资源是数据库读取操作，或者进行一些数据处理，在服务器端用相应的模版对页面进行渲染，最后返回渲染完毕的页面

服务器端路由的**优点**是安全性好，利于seo；**缺点**是加大服务器压力，不利于用户体验，代码冗余不好维护。
### 前端路由
在单页面应用中，前端的页面逻辑复杂，所以使用前端路由方便切换页面，从用户角度看，前端路由主要实现两个功能：
- 记录当前页面的状态（保存当前url，再次打开url时，网页还是保存着刚才的状态）
- 可使用浏览器的前进后退功能

在开发者角度，要做到：
- 改变 url 但不让浏览器向服务器发出请求
- 检测 url 的变化
- 截获 url 地址，并解析出需要的信息匹配路由规则

前端路由的**本质**是：根据路由的映射函数进行一些dom的显示和隐藏操作。
实现路由功能，有两种方法：hash模式 和 html5提供的history模式
### 路由模式一：hash模式
hash 是指 url 最后的#号以及后面的字符，这也是css中的锚点，本来是超链接`a`加锚点实现页面中位置跳转，如点击章节名跳转到对应位置。

hash 就可以实现一个[简单的单页面应用](https://github.com/Naturalvolume/IFE-2016Spring/blob/master/SPA.html)， 这是因为hash具有以下特点：
- url 中的 hash值只是客户端的一种状态，**发送请求时不会带hash值**，所以hash 值变化不会导致浏览器向服务器发送请求
- hash 值变化会触发 haschange 事件
- hash 值改变，会在浏览器的访问历史中增加一个记录，所以浏览器的前进后退能控制 hash 的切换

有两种方式可以触发 hash 变化：
- 设置`a`标签的href属性，当用户点击这个标签后，`url`改变，就会触发`hashchange`事件
- 直接使用 js 改变 `window.location.hash` 的值，触发`hashchange`事件
```javascript
// a 标签方法
<a href="#search">search</a>
// js方法
window.location.hash = 'qq' // 设置 url 的 hash，会在当前url后加上 '#qq'
 
window.addEventListener('hashchange', function(){ 
    // 监听hash变化，点击浏览器的前进后退会触发
})
```
**使用场景**：hash 模式常用于开发阶段，方便开发
**限制**：不利于seo，因为一个hash不是一个新的url地址，不会收录；基于url的，不利于传参，复杂数据有体积限制。
### 路由模式二：history
history 是**浏览器历史记录栈提供的接口**，通过`back()`、`forward()`、`go()`等方法，可以读取浏览器历史记录栈信息，进行各种跳转操作。

在html5中，新增了两个 history 方法：`pushState()`、`replaceState()`可以在**不刷新页面**的情况下，实现对浏览器历史记录栈的修改.
```javascript
// 新增一个历史记录
window.history.pushState(stateObject,title,url)
// 替换当前的历史记录
window.history.replaceState(stateObject,title,url)
```
- stateObject: 当浏览器跳转到新的状态时，将触发popState事件，该事件将携带这个stateObject参数的副本
- title: 所添加记录的标题
- url: 所添加记录的url
当调用这两个方法修改浏览器历史栈后：
- 它们的标题（title）属性，一般浏览器会忽略，最好传入`null`
- 用 `popstate`事件监听 url 的变化
- 它们不会触发`popstate`事件，需要手动触发页面渲染
**使用场景**：hash 模式常在上线之后使用，history 有利于seo，且url简洁美观
**限制**：需要服务器端对路由进行相应配合设置

#### 参考：[深度剖析：前端路由原理](https://juejin.im/post/5d469f1e5188254e1c49ae78#heading-11)


