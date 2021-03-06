# 访问权限控制
权限控制在后端MVC时代，是由后端程序员在 jsp 或 php模版中实现的。后来考虑到前后端开始解藕 及 下列问题，开始使用前端权限控制：
- 提升突破权限的门槛，防范掉 手动输url、控制台发请求、开发者工具改数据 这种级别的入侵
- 过滤越权请求，减轻服务端压力
- 提升用户体验

### 前端权限控制的具体目标
- 路由：用户登陆后只能看到自己有权访问的导航菜单，也只能访问自己有权访问的路由地址，否则将跳转到 4** 提示页
- 视图：用户只能看到自己有权浏览的内容和有权操作的控件
- 请求控制：因为路由可能配置失误、按钮可能忘了加权限，这时候请求控制可以将越权请求在前端被拦截

### 前端控制
第一步是知道用户拥有哪些权限，所以用户登陆后第一件事就是获取权限数据：
- 路由权限：用户可访问的路由集合，作为**设置前端路由**和**生成导航菜单**的依据
- 资源权限：用户可访问的资源集合（Restful架构中，可发起请求的集合），作为**视图控制**和**请求拦截**的依据

##### 路由控制
有两种思路
- 初始化时挂载全部路由，每次路由跳转前做校验，缺点是 每次路由跳转前都要计算校验，造成资源浪费
- 只挂载用户拥有的路由，相当于从源头上做控制，这样就需要用户先登陆验证
  - 单独做一个登陆页，登陆后带着用户凭据跳转到前端应用
  - 先初始化只有登陆路由的应用，用户登陆后动态添加路由，如vue的`router.addRoutes`

```javascript
if(token) {
  // 根据用户角色生成对应的路由
  const newRouter = [{path: "/***" ...} ..]
  // 添加新路由到路由中
  router.addRoutes(newRouter)
  // 为了正确渲染导航，将对应的新路由添加到vuex中，渲染对应的侧边栏
}
```
##### 视图控制
实现一个可以在视图层调用的权限验证方法，输入用户期望的权限，输出是否拥有该权限，调用这个方法的结果，作为界面上需要验证权限的控件或元素显示与否的依据

##### 请求控制
使用http库实现请求拦截器，对将要发起对请求与用户资源权限进行匹配，拦截越权请求。

对于带参数的url，需要先进行模式约定，比如`/people/1`可以在权限中描述为`/people/**`，那么拦截器先将这种url处理成约定后的格式，再进行权限验证。

### 访问控制的几种情况
- axios 请求需要token时，可以配置请求拦截器
- 有些页面需要登陆才能看，可以用路由导航守卫`router.beforeEach`判断token
- 根据不同的用户角色，决定页面要展示的内容，一般的思路是 比如侧边栏是路由，需要循环生成，根据不同的人路由数组不一样，循环生成不同的侧边栏

参考：[前端权限控制](https://blog.csdn.net/yin_you_yu/article/details/80408340)



