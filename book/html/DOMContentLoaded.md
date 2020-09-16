# async、defer与DOMContentLoaded的执行先后关系
### HTML解析过程与DOMContentLoaded触发时机
DOMContentLoaded的触发时间为：
- html中无css、js：当 DOM 树被加载和解析完成，在生成render tree之前触发
- html中有css：当 DOM 树被加载和解析完成时触发，不管css树是否解析加载完成，只看dom树（也是在生成render tree之前）
- html中有js：碰到script标签时，先加载和执行js，等执行完成继续解析完dom后再触发，所以js会在触发 DOMContentLoaded 之前执行

### 有async属性的script时
DOMContentLoaded 可能会在js执行之前或之后触发，因为DOMContentLoaded就是在dom树加载完成后触发
### 有defer属性的script时
DOMContentLoaded 会在js执行之前触发，因为DOMContentLoaded就是在dom树加载完成后触发

参考：[async、defer与DOMContentLoaded的执行先后关系](https://blog.csdn.net/zyj0209/article/details/79698430)

### window.OnLoad 和 DOMContentLoaded 触发的顺序
一般来讲 DOMContentLoaded 会在 window.OnLoad 之前触发，因为
- window.OnLoad 是在页面上所有的dom、css、js、图片都加载完成后触发
- DOMContentLoaded 只是dom加载完成后触发