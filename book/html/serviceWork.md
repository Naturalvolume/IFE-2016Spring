# 浏览器缓存机制——ServiceWork
`ServiceWorker`是浏览器在后台独立于网页运行的脚本，可理解为浏览器和服务端之间的**代理服务器**，可实现推送通知、后台同步、离线缓存等功能。

#### 使用限制
浏览器对`ServiceWorker`做了许多限制：
- 在`ServiceWorker`中无法直接访问dom，可通过`postMessage`接口发送的消息来与其控制的页面进行通信；
- `ServiceWorker`只能在**本地环境下**或`https`网站中使用；
- `ServiceWorker`有作用域的限制，一个`ServiceWorker`脚本只能作用于当前路径及其子路径；
- `ServiceWorker`有兼容性问题。

#### `ServiceWorker`的使用
通过拦截浏览器请求并返回缓存的资源文件实现浏览器缓存。

```javascript
// 步骤一：先判断 window.navigator 中是否存在 serviceWorker 属性
if ('serviceWorker' in window.navigator) {
  window.navigator.serviceWorker
// 步骤二：调用register函数告诉浏览器serviceWorker脚本的路径
    .register('./sw.js')
    .then(console.log)
    .catch(console.error)
} else {
  console.warn('浏览器不支持 ServiceWorker!')

const CACHE_NAME = 'ws'
let preloadUrls = ['/index.css']
// 步骤三：监听"install"事件，这个事件只在第一次加载脚本时触发
// 步骤四：激活脚本
self.addEventListener('install', function (event) {
  event.waitUntil(
    caches.open(CACHE_NAME)
    .then(function (cache) {
      return cache.addAll(preloadUrls);
    })
  );
});
// 步骤五：监听fetch事件来拦截请求并加载缓存的资源
self.addEventListener('fetch', function (event) {
  event.respondWith(
    caches.match(event.request)
    .then(function (response) {
      if (response) {
        return response;
      }
      return caches.open(CACHE_NAME).then(function (cache) {
          const path = event.request.url.replace(self.location.origin, '')
          return cache.add(path)
        })
        .catch(e => console.error(e))
    })
  );
})
```