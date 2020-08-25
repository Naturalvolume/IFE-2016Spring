# webpack中的热更新和热加载
- 热刷新：文件内改动后，整个页面刷新，不保留任何状态（比如输入过内容的input表单）
- 热加载：文件改动后，以最小的代价改变被改变的区域，尽可能保留该懂文件前的状态（对input输入内容后，修改其它标签的代码）
这两者虽然相较于传统开发（手动F5）都提升了开发体验，但两者间但差异依旧很大，尤其是当项目变得愈发复杂后，可以一劳永逸的解决这个问题。

### webpack热刷新
是webpack-dev-server启动一个socket服务，监听webpack打包文件变化，有文件更新时webpack-dev-server通知浏览器某个文件变化了，浏览器就会刷新当前页面。
```javascript
devServer: {
  contentBase: './dist',
  port: '7008',
  inline: true,
  host: '0.0.0.0'
  disableHostCheck: true, // 关闭 Host 检查
  // allowedHosts: ['你的域名'] // 白名单
}
```
### webpack热加载
在热刷新的基础上，增加更多配置
```javascript
devServer: {
  contentBase: './dist',
  port: '7008',
  inline: true,
  host: '0.0.0.0',
  disableHostCheck: true,
  // 开启热加载
  hot: true,
}
```
- 用socket监听到修改文件后
  - 触发reloadAPP()
    - 未开启热更新：刷新浏览器
    - 开启热更新：触发webpackHotUpdate事件
      - 检查webpack Hash和前端收到的hash是否相同，检查不成功就继续检查，直到成功
      - 热更新失败，刷新浏览器