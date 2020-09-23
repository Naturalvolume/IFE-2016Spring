# WebSocket
html5提供了可用于实时直播通信的解决方案——`socket.io`，之前都是使用ajax长短轮训或其他的。

WebSocket是html5开始提供的一种在单个tcp连接上进行全双工通讯的协议，可以更好的**节省服务器资源和带宽**，并且能够更**实时地进行通讯**。
#### 创建WebSocket对象
```javascript
var Socket = new WebSocket(url, [protocol] )
```

#### 属性
- Socket.readyState: 表示连接状态
  - 0: 尚未建立
  - 1: 连接已建立，可进行通信
  - 2: 连接正在进行关闭
  - 3: 连接已经关闭或连接不能打开
- Socket.bufferedAmount: 只读属性`bufferedAmount`已被 send() 放入正在队列中等待传输，但是还没有发出的 UTF-8 文本字节数

#### 事件
- Socket.onopen: 连接建立时触发
- Socket.onmessage: 客户端接收服务端数据时触发
- Socket.onerror: 通信发生错误时触发
- Socket.onclose: 连接关闭时触发

#### 方法
- Socket.send(): 使用连接发送数据
- Socket.close(): 关闭连接

