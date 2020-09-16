# 实现一个ajax请求
### XMLHttpRequest
ajax的核心是 XMLHttpRequest，完整的ajax请求包括以下几个步骤：
- 实例化 XMLHttpRequest
- 连接服务器
- 发送请求
- 接收响应数据

### XMLHttpRequest的方法
- xhr.open(method, url, async)：发送请求
- xhr.readyState：返回当前请求状态
  - 0-(未初始化) 还没有调用send()方法
  - 1-(启动) 调用open()方法发送请求
  - 2-(发送) 调用send()方法  
  - 3-(接收) 接收到部分响应内容
  - 4-(完成) 响应内容解析完成，可以在客户端调用了
- xhr.status：http状态码
- xhr.onreadystatechange()：readyState改变时，会触发onreadystatechange 事件

### 发送请求
- get请求
请求参数拼在url上面

```javascript
var xhr=new XMLHttpRequest();//实例化
xhr.open('get','/abc?username=lili',true);
xhr.send();				//将请求发送到服务器
xhr.onreadystatechange = function(){
    if(xhr.readyState === 4){
        if(xhr.status === 200){
            $('.h1').html(xhr.responseText);
        }else{
            $('.h1').html('ERROR');
        }
    }
}
```
### post请求
请求参数放到send中，即请求主体

```javascript
var xhr=new XMLHttpRequest();
xhr.open('post','/abc',true);
xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
xhr.send('username=weiwei&age=19');
xhr.onreadystatechange = function() {
    if(xhr.readyState===4){
        if(xhr.status===200){
            console.log(JSON.parse(xhr.responseText));
        }else{
            console.log('ERROR');
        }
    }
}
```

### 封装ajax
```javascript
function ajax(options) {
  
  options = options || {}     // 没有传入参数时，就指定参数
  options.type = (options.type || 'GET').toUpperCase()  // 请求方式默认为大写
  options.dataType = options.dataType || 'json'   // 设置响应数据格式，默认为json

  var params = formatParams(options.data)   // 格式化请求数据

  var xhr
  // 兼容性判断
  if(window.XMLHttpRequest) {
    xhr = new XMLHttpRequest()
  } else if(window.ActiveXObject) {   //兼容ie7以下版本
    xhr = new ActiveXObject('Microsoft.XMLHTTP')
  }

  // 开始发送请求
  if(options.type == 'GET') {
    // 发送请求，设置异步
    xhr.open('GET', options.url + '?' + params, true)
    xhr.send(null)  // 内容主体为null
  } else if(options.type == 'POST') {
    xhr.open('POST', options.url, true)

    // 设置表单提交的内容类型
    xhr.setRequestHeader('Content-type', "application/x-www-form-urlencoded")
    xhr.send(params)  // post请求，数据放到主体
  }

  // 设置有效时间
  setTimeout(function() {
    // 已经到达请求时间还未请求完毕
    if(xhr.readyState != 4) {
      // 关闭请求
      xhr.abort()
    }
  }, options.timeout)
  // readyState状态改变就会触发这个事件
  xhr.onreadystatechange = function() {
    console.log('change')
    // 判断数据是否传输完毕
    if(xhr.readyState == 4) {
      var status = xhr.status
      // 请求成功，调用成功回调函数
      if(status >= 200 && status<300 || status==304) {
        options.success && options.success(xhr.responseText, xhr.responseXML)
      } else {
        // 失败，调用失败回调函数
        options.error && options.error(status)
      }
    }
  }
}

// 格式化请求参数
function formatParams(data) {
  var arr = []
  for(var name in data) {
    arr.push(encodeURIComponent(name) + '=' + encodeURIComponent(data[name]))
  }
  // 设置随机值
  arr.push(('v=' + Math.random()).replace('.', ''))
  return arr.join('&')
}

// 使用ajax
ajax({
  url: 'http://baidu.com',
  type: 'get',
  data: {
    username:'username',
    password:'password'
  },
  dataType:'json',
  timeout:10000,
  contentType:"application/json",
  success: function() {
    console.log('success')
  }, 
  error: function() {
    console.log('error')
  }
})
```

参考：[原生js实现ajax封装](https://www.cnblogs.com/qing-5/p/11368009.html)
### 关于跨域请求
jsonp的实现：[原生JavaScript实现AJAX、JSONP](https://laixiazheteng.com/article/page/id/AASiankfBJWp)

### fetch
fetch 基于promise实现的**原生js方法**，没有使用XMLHttpRequest方法，可以用promise的形式或async/await的形式实现异步调用，

```javascript
// fetch(url, options)
fetch('http://www.mozotech.cn/bangbang/index/user/login', {
  method: 'post',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  ,
  body: new URLSearchParams([["username", "Lan"],["password", "123456"]]).toString()
})
.then(res => {
  console.log(res);
  return res.text();
})
.then(data => {
  console.log(data);
})
```

fetch 发送请求时，不像XHR，默认是不带cookie的，加上`credentials: include`就会在发送请求时带上cookie了。
### axios
axios是对**原生XHR对象**的***promise**实现，可用于浏览器和nodejs的http客户端。

```javascript
axios({
  method: 'post',
  url: '/abc/login',
  data: {
    userName: 'Lan',
    password: '123'
  }
})
.then(function (response) {
  console.log(response);
})
.catch(function (error) {
  console.log(error);
});
```

### ajax、axios、fetch对比
- ajax：原生支持，不需要任何插件，但嵌套回调，难以处理。
- fetch: 解决回调地狱，使用也更加简洁，是原生js，所以使用起来不太友好，需要手动添加cookie，浏览器支持情况不好，ie浏览器和部分手机浏览器完全不支持。
- axios: 防止CSRF，自动转换json数据，是目前最完美的实现。
[ajax和axios、fetch的区别](https://www.jianshu.com/p/8bc48f8fde75)


