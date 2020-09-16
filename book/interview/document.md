# 页面的加载顺序
- 构建dom树，创建document对象，解析文档节点，此时`document.readyState = 'loading'`
- 遇到link引入的css标签，创建线程下载css文件，不阻塞dom的解析
- 遇到script标签
  - 下载并执行js，阻塞dom的解析
  - 有async属性的script，异步加载js，加载完成后立刻执行js，阻塞dom解析
  - 有defer属性的script，推迟加载js，dom树解析完成后开始加载并执行js
- 遇到含有src属性的其他标签，如`<img>`，异步加载src，继续解析dom结构
- dom树构建完毕，此时`document.readyState='interactive'`，触发`DOMContentLoaded`事件，标志着**程序执行由同步脚本执行阶段转换为事件驱动阶段**
- 有defer属性的js脚本执行
- 页面加载渲染完成，状态改为`document.readyState='complete'`，触发`window.onLoad()`事件
- 此后，以异步响应方式处理用户输入、网络事件等

### 下载css和js的影响
- 现代浏览器会在允许范围内，并行发送下载请求，但是按照书写顺序执行代码
- 加载和执行js会阻塞 dom 树的渲染，因为dom树形成、js执行、页面渲染都是一个线程
- css 下载会阻塞后面js的执行，不会阻止位于它前面的
- js会阻止dom的解析，但css不会阻止dom的解析
- css会阻止页面的渲染，因为要生成render树后才会渲染页面