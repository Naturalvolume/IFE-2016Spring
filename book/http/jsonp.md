# 实现jsonp
jsonp是JSON with Padding的简称。

jsonp 是利用**src属性**实现跨域的一种方式（src标签不被同源策略所约束），浏览器端在`url`中添加回调函数参数，服务器端将json数据和回调函数拼接成字符串一起返回给浏览器。
### js实现
实现步骤如下：
- 请求：浏览器创建一个 script 标签，并给其src 赋值(类似 `http://example.com/api/?callback=jsonpCallback`）
- 发送数据：给 script 的 src 赋值时，浏览器就会发起请求
- 数据响应：服务端将**要返回的数据**作为参数和**回调函数**拼接，类似于`jsonpCallback({name: 'abc'})`，浏览器接收到响应数据

### 简单实现
浏览器
```javascript
function addScriptTag(src) {
  var script = document.createElement('script')
  script.setAttribute('type', 'text/javascript')
  script.src = src
  document.body.appendChild(script)
}
// 网页全部加载完成后
window.onload = function() {
  // 将定义的回调函数，放到请求url的callback参数中
  addScriptTag("http://localhost:3000/callback=result")
}
// 自定义回调函数result
function result(data) {
  let res = data.message
}
```

服务器端做解析url，添加数据到回调函数中，且返回的回调函数的操作。

[彻底弄懂jsonp原理及实现方法](https://www.cnblogs.com/soyxiaobi/p/9616011.html)
### [原生JavaScript实现AJAX、JSONP](https://laixiazheteng.com/article/page/id/AASiankfBJWp)


