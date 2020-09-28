# 存储用户信息
### session
##### session的销毁
- 到了过期时间之后被销毁（session的有效期通常较短，普通设置就是20分钟，所以20分钟后用户和服务器端没有交互就会被删除）
- 关闭客户端的时候
- 手动销毁
浏览器关闭时，原session并没有被销毁，而是等到session过期，关闭浏览器只是在客户端内存中清除了与原会话相关的cookie，再次打开浏览器进行连接时，浏览器无法发送cookie信息，所以服务器会认为是一个新的会话，所以对于某些与session关联的资源**要在关闭浏览器时就进行清理（如临时文件等）**，就要**手动**发送特定请求到服务器端，而不是等到session自动清理。

##### session与内存
session数据**无法跨进程共享** 且 **会占用过多内存**，所以把session**集中化存储**，将原本可能分散在多个进程中的数据，统一转移到集中的数据存储中。目前常用的工具是Redis、Memcached等，这些高效缓存，具有：
- 无需在内部维护数据对象
- 解决垃圾回收问题和内存限制问题
- 缓存过期策略更好

### cookie
客户端设置cookie 
```javascript
document.cookie = 'username = zs; expires = 过期时间'
```

cookie默认浏览器关闭就过期了，但是可以自己设置过期时间，且不太安全（因为请求的时候每次都会带上）。

cookie是很久以前的技术，那时候用来存储用户登陆(cookie跨域有问题)，现在**用localStorage存token来操作**。

### 离线缓存 manifest
在`<html>`标签中设置`manifest`属性，表示配置存储的文件
```html
<html manifest = 'cache.manifest'>
```

```manifest
<!-- 版本 -->
CACHE MANIFEST
#v0.11

<!-- 表示要离线存储的资源，不需要单独列出页面自身 -->
CACHE: 
js/app.js
css/style.css

<!-- 表示只能在在线情况下才能访问的资源，他们不会被离线存储 -->
NETWORK:
resource/logo.png

<!-- 表示若访问上面的资源失败，就用下面的资源替换 -->
<!-- 比如下面的资源就是 若访问根目录下任何一个资源失败了，就去访问offline.html -->
FALLBACK: 
offline.html
```

