# websocket
websocket 是 html5 新出的协议，并没有改变http协议，但借用http协议完成了一部分握手，即在握手阶段是一样的。
### 握手阶段
先用http完成握手阶段，升级协议到 websocket:
- 客户端在http请求头中提出升级协议到 websocket

```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://example.com
```

`upgrade` 和 `connection` 告诉服务端要建立起 websocket 协议；`Sec-WebSocket-`定义了websocket的版本等信息。
- 服务器返回下列响应头，表示接收到请求，且成功建立 websocket
```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
```

接下来按照websocket协议进行通信，建立持久连接

### 编写websocket代码
客户端支持webSocket的浏览器中，建立socket后，可以通过一系列时间对socket进行响应，WebSocket的所有操作都采用**事件**的方式触发。
```javascript
// 申请一个WebSocket对象，参数是服务端地址
var ws = new WebSocket('ws://localhost:8080')

// webSocket创建成功，触发onopen事件
ws.onopen = function() {
  // 向服务端发送消息
  ws.send("message")
}
// 客户端收到服务端发来的消息，触发onmessage事件
ws.onmessage = function(e) {
  console.log(e.data)
}
// 客户端收到服务端发送的关闭连接请求，触发onclose事件
ws.onclose = function(e) {}
// 有错误出现时
ws.onerror = function(e) {}
```

### socket.io
socketIO 将webSocket、ajax和其它通信方式全部封装成了统一的通信接口，所以在使用SocketIO时，不需要担心兼容问题，底层会自动选用最佳的通信方法。

客户端和服务端的socketIO实现
##### 客户端实现
```javascript
// 在<head>标签中引入 socket.io.js文件

// 创建webSocket实例
var socket = io()
// 发送自定义事件给后端
socket.emit('join', name)
// 监听后端的自定义事件
socket.on('join', function() {

})
```

### 服务端实现

```javascript
var app = require('express')()
var http = require('http')
var io = require('socket.io')(http)
```