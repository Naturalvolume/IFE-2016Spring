### 1. 堆叠上下文问题
场景一：导航栏
场景二：歌单详情页，专辑封面设置高斯模糊和灰度效果

父元素设置背景和高斯滤波效果，增加子元素绝对定位设置rgba背景
```css
  .background {
    z-index: -1;
    background: url(${props => props.background}) no-repeat;
    background-position: 0 0;
    background-size: 100% 100%;
    position: absolute;
    width: 100%;
    height: 100%;
    // filter是css3新增的滤镜效果，blur表示高斯模糊
    filter: blur(20px);
    // 这里跟上面的filter是不一样的，它的作用是给调节背景图片色调
    // 疑惑：这里为什么可以重叠两个背景
    .filter {
      position: absolute;
      z-index: 10;
      top: 0; left: 0;
      width: 100%;
      height: 100%;
      background: rgba(7, 17, 27, 0.2);
    }
  }
```


### 2. 详情页路由跳转问题（动态路由）
在推荐页面跳转歌曲详情页面时，先在路由配置文件中设置子路由
```javascript
import Album from '../application/Album';

// 在 /recommend 后面加上子路由
{
  path: "/recommend",
  component: Recommend,
  routes: [
    {
      path: "/recommend/:id",
      component: Album
    }
  ]
},
```
然后用更改url，实现动态路由跳转，动态路由有两种实现方式
```javascript
// <Redirect/>标签
<Redirect to="/home/" />

// history方法
// history是一个管理session会话历史的js库，将不同环境（浏览器、node）的变量统一成一个简易的api管理历史堆栈，导航、确认跳转、以及session之间的持续状态
props.history.push("/home/");  
```
**问题一**：本项目使用history方法实现动态路由，在推荐页面的子组件列表<List>组件中实现更改history，但是子组件不能从props拿到history变量，无法跳转路由，有两种解决方法。
```javascript
// 1. 通过props传值
// 2. 高阶组件，用withRouter包裹List组件
//List/index.js
import { withRouter } from 'react-router-dom';

// 省略组件代码

// 包裹
export default React.memo (withRouter (RecommendList));
```
**问题二**：实现跳转页面后，歌单详情并没有被渲染出来，这是因为？
不是因为堆叠上下文问题，而是没有在<List>中加入渲染子路由逻辑，无法解析路由配置
```javascript
// 返回的 JSX
<Content>
  // 其他代码
  // 将目前所在路由的下一层子路由加以渲染
  { renderRoutes (props.route.routes) }
</Content>
```