# 路由懒加载
在react 中引入了react lazy实现动态路由的加载
```javascript
// 1. 引入react lazy，并且使用import动态导入组件
import { lazy } from 'react'  // 静态导入，js刚执行的时候 就已经把它导入进来了

lazy(() => import('./home'))  // 动态导入，当代码执行到这里时，才开始下载组件

// 2. 引入suspense组件，并用suspense包裹根组件
// 使用 fallback props传入loading组件
// 即加载按钮
<Suspense fallback={<div> Loading...</div>}>
  <OtherComponent/>
</Suspense>
```

当webpack解析到动态加载的语法时，会自动进行代码分割。如果使用`create react app`，该功能已经开箱即用，可以立刻使用该特性，Next.js 也支持该特性而无需进行配置。

当使用 babel 时，要确保 babel 能够动态解析 import 语法而不是将其进行转换，这样就需要使用`babel-plugin-syntax-dynamic-import`插件。

动态路由返回的是一个promise，所以可以用promise的状态来做渲染的流程控制，若当前promise是 pending 状态，那么就渲染 Loading 组件，如果是 resolve 状态那么就渲染动态导入的组件。