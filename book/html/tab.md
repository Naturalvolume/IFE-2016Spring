# 跨页面通信的方式
### localStorage
localStorage 是在本地存储某个域下信息的一种方式，属于同一个域的网页可以通过`localStorage.setItem(key, value)`和`localStorage.removeItem(name)`对它进行**增删改查**操作，同时触发同一个域下其它页面的`storage event`事件，在事件event对象属性中获取信息，event事件对象包含以下信息：
- domain
- newValue
- oldValue
- key

storage event 是在同一个域下的不同页面间触发，如A页面注册了`storage`监听处理，那么在与A页面同名的B页面下操作storage对象，A页面才会触发`storage`事件。

参考：
[监听localStorage变化](https://www.jianshu.com/p/519a1b42d659)

[Window.localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)

### cookie + setInterval()
将要传递的信息存储在cookie中，每隔一定事件读取cookie信息，即可随时获取要传递的信息。（只有在服务器环境中才算是同一个域名）

- 在A页面将要传递的信息存储在cookie中

```javascript
<input id="name">
<input type='button' id='btn' value='提交'>
<script type='text/javascript'>
  window.onload = function() {
    var btn = document.getElementById('btn')
    var nameEle = document.getElementById('name')
    btn.onclick = function() {
      var name = nameEle.value
      // 增加cookie的值
      document.cookie = 'name=' + name
    }
  }
</script>
```

- 在B页面设置setInterval，以一定时间间隔读取cookie的值

```javascript
window.onload = function() {
  function getCookie(key) {
    if(document.cookie) {
      return JSON.parse(document.cookie)
    } else {
      return ""
    }
  }
  setInterval(function() {
    getCookie()
  }, 5000)
}
```

### webSocket + 服务器中转
webSocket可以实现客户端和服务器的全双工的通讯，要让多个标签页通信，可以通过先给服务器发信息，再发给其它标签页的形式（类似于兄弟组件通信中的变量提升）。

### web Worker