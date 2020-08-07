# axios
axios基于原生 XMLHttpRequest 和 promise 实现，具有以下特征：
- 从浏览器和nodejs发送http请求
- 支持promise(.all 可执行多个并发请求)
- 拦截请求和响应(axios.interceptors.request.use())

应用场景：判断每个请求都带上的参数，比如token、时间戳等；对返回的状态进行判断，如token是否过期

- 转换请求和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端防止CSRF/XSS

在本项目中用 axios.create 创建axios的实例，配置拦截器将数据从res变为res.data.