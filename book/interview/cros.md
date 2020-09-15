# 跨域
### 为什么要有跨域问题
出于安全原因，浏览器限制从脚本内发起的跨域http请求，例如XMLHttpRequest和Fetch API遵循同源策略（即只能从加载应用程序的同一个域请求），或浏览器拦截返回结果，除非响应报文包含正确的CORS响应头。

### CORS 跨域资源共享
是一种机制，使用额外的http头告诉浏览器让运行在一个 origin domain 上的web应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源发起一个**跨域http请求**。

比如：
- 站点 `http://domain-a.com` 的某 HTML 页面通过 `<img>` 的 src 请求 `http://domain-b.com/image.jpg`
- 网络上的许多页面会加载来自不同域的css样式表、图像和脚本等资源

现代浏览器支持在API容器中（如 XMLHttpRequest和Fetch）使用CORS，降低跨域http请求所带来的风险。

### 跨域http请求场景
- XMLHttpRequest 和 Fetch发起的跨域http请求
- web字体（css中通过@font-face使用跨域字体资源），网站发布字体资源，并只允许已授权网站进行跨站调用
- webGl贴图
- 使用 drawImage 将 images/video画面绘制到canvas

### 需要预检请求的场景
- get以外的http请求
- 搭配某些MIME类型的post请求
以上是会对服务器数据产生副作用的http请求方法，浏览器必须首先使用options方法发起一个预检请求，获知服务端是否允许该跨域请求（服务器通过新增的http首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源），服务器确认允许后，才发起实际的http请求，在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括cookies和http认证相关数据）。

### 跨域资源共享机制
#### 例子一：简单请求
简单请求（get、post、head）不会触发CORS预检请求，还有一些对CORS安全的首部字段集合（允许默认头部使用）：
- Accept
- Accept-Language
- Content-Language
- Content-Type
- DPR
- Downlink
- Save-Data
- Viewport-Width
- Width
- Content-Type 值仅限于以下三者
  - text/plain
  - multipart/form-data
  - application/x-www-form-urlencoded

对于简单请求，浏览器直接发出一个CORS请求，并在请求头中加入一个Origin段来描述本次请求来自哪个源（协议＋域名＋端口），服务器在返回头中返回包含`Access-Control-Allow-Origin`段，表示哪些外域可以访问：
- `Access-Control-Allow-Origin: *`   表示该资源可以被任意外域访问（不能携带cookie）
- `Access-Control-Allow-Origin: http://foo.example `   表示只能被这个网址访问，其它外域不可以
#### 例子二：预检请求
若不是简单请求，就要对请求先使用 options 方法确认服务器是否允许该请求，添加如以下的请求头：
- Access-Control-Request-Method: POST
- Access-Control-Request-Headers: X-PINGOTHER, Content-Type
服务器返回：
- `Access-Control-Allow-Origin: http://foo.example`
- Access-Control-Allow-Methods: POST, GET, OPTIONS
- Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
- Access-Control-Max-Age: 86400    表示在24小时内，无需为同一请求再次发起预检请求

##### 注意：
- 预检请求之后，再发送实际请求，不需要再携带预检请求头
- 浏览器不支持预检请求的重定向，否则会报错

#### 附带身份凭证的请求
CORS 的 XMLHttpRequest 和Fetch 可以基于cookies发送身份凭证，默认的跨域请求并不会发送身份凭证，需要设置XMLHttpRequest的特殊标志位 withCredentials 为 true，且服务器响应Access-Control-Allow-Credentials: true 和 `Access-Control-Allow-Origin：http://foo.example` **（不能是*）**才是请求成功。
### localStorage的跨域存储
同一浏览器的相同域名和端口的不同页面间可以共享相同的loaclStorage，但是不同页面间无法共享sessionStorage的信息（iframe的同源页面可以共享）。

有时会遇到，不同源页面间需要共享localStorage的要求 或者 某一源页面localStorage容量不够，可以使用postMessage(data, origin)和iframe相结合的方法 对 不同源对脚本采用异步方式通信，实现跨文本档、多窗口、跨域消息传递。

参考：
[localstorage的跨域存储方案](https://www.jianshu.com/p/e86d92aeae69)
[localstorage跨域解决方案](https://blog.csdn.net/sflf36995800/article/details/53290457)