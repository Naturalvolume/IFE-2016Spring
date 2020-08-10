### 网易云webapp
以react全家桶和immutable数据流为基础打造的一款移动端音乐类webapp.

技术栈包括 react v16.8全家桶（react、react-router）、redux、redux-thunk、immutable、react-layload、swiper(移动端触摸事件做轮播图)、better-scroll(移动端滑动基础组件)、style-component、axios、fastclick(解决移动端点击延迟300ms问题)，使用github上开源的nodejs版网易云音乐接口。

特点：
- 不使用class组件，使用函数式组件和hooks实现生命周期功能
- 业务逻辑数据全部放到redux中管理
- ajax请求以及后续数据处理的具体代码全部放在actionCreator中，由redux-thunk处理
- 每一个容器组件都有自己独立的reducer，然后再全局的store下通过redux的combinReducer方法合并
- 普通css类名全部小写，单词用下划线连接，css动画钩子类名称用 - 连接
- 要从props中取数据的，全部在组件最前面解析结构，属性名方法名分开声明，父组件取得的和redux中映射到的也分开声明
- 每个组件都用memo包裹，使得react在更新组件之前进行props比对，props不变则不对组件更新，减少不必要的重渲染

### 为什么要用immutable
immutable和react.memo结合，提高渲染性能
### 为什么要用style-components
- 符合react全员js的思想，不需要组件和样式之间的映射，创建后就是一个正常的react组件，直接在JSX中引入即可
- 虽然结构、样式、逻辑不分离，但是**有利于组件间的隔离**
- 可以把全局样式风格(如颜色、字体等)抽离在一个单独的文件，使用时用import引入，然后`${style["background-color"]}`使用样式。
- 可以从props中传入数据，如把从后端传过来的专辑封面显示出来，在js文件中通过`<TopDesc background={currentAlbum.coverImgUrl}>`把coverImgUrl设置到背景上，然后在style.js中，`background: url(${props => props.background}) no-repeat;`可以取到props的值。