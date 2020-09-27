# axios
axios基于原生 XMLHttpRequest 和 promise 实现，具有以下特征：
- 从浏览器和nodejs发送http请求
- 支持promise(`axios.all` 可执行多个并发请求)

```javascript
function getUserAccount() {
  return axios.get('/user/12345');
}
 
function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}
 
axios.all([getUserAccount(), getUserPermissions()])
.then(axios.spread(function (acct, perms) {
    //两个请求现已完成
}));
```
- 拦截请求和响应(axios.interceptors.request.use())

应用场景：判断每个请求都带上的参数，比如token、时间戳等；对返回的状态进行判断，如token是否过期

- 转换请求和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端防止CSRF/XSS：通过设置`xsrfCookieName: 'XSRF-TOKEN'`用作 xsrf token，axios 让每个请求都带一个从`cookie`中拿到的`key`，根据浏览器的同源策略，假冒的网站是拿不到`cookie`中`key`的，后台可以辨别这个请求是否在用户假冒网站上的误导输入，从而采取正确的策略。
- 不支持`JSONP`，但是可以自己封装：

```javascript
//axios本版本不支持jsonp 自己拓展一个
axios.jsonp = (url) => {
    if (!url) {
        console.error('Axios.JSONP 至少需要一个url参数!')
        return;
    }
    return new Promise((resolve, reject) => {
        window.jsonCallBack = (result) => {
            resolve(result)
        }
        var JSONP = document.createElement("script");
        JSONP.type = "text/javascript";
        JSONP.src = `${url}&callback=jsonCallBack`;
        document.getElementsByTagName("head")[0].appendChild(JSONP);
        setTimeout(() => {
            document.getElementsByTagName("head")[0].removeChild(JSONP)
        }, 500)
    })
}
```

在本项目中用 axios.create 创建axios的实例，配置拦截器将数据从res变为res.data

### 封装一个axios
定义一个`axios.js`文件

```javascript
import axios from 'axios'
import qs from 'qs'
// 设置请求的服务器地址
axios.defaults.baseURL = 'https://127.0.0.1:3000'
// 设置请求超时时间
axios.defaults.timeout = 10000
// 设置为请求携带 cookie
axios.defaults.withCredentials = true
// 设置请求传递数据的格式（根据服务器要求的格式）
axios.defaults.headers['Content-Type'] = 'application/x-www-form-urlencoded'
axios.defaults.transformRequest = data => qs.stringify(data)

// 设置请求拦截器
// token校验（用JWT编码）：接收服务器返回的token，存储到vuex/本地
// 每次向服务器发请求，带上token
axios.interceptors.request.use(config => {
  let token = localStorage.getItem('token')
  // 存在token，设置请求头携带token
  token && (config.headers.Authorization = token)
  return config
}, error => {
  return Promise.reject(error)
})

// 响应拦截器
axios.interceptors.response.use(response => {
  return response.data
}, error => {})

export default axios
```


### 数据请求的时间
- 用useEffect在第一次渲染时发送http
- 在dispatch中