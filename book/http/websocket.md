# websocket
websocket 是 html5 新出的协议，并没有改变http协议，但借用http协议完成了一部分握手，即在握手阶段是一样的。
### 连接过程
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