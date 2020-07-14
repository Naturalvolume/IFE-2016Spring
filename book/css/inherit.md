## css属性的继承
继承主要指文本方面的继承（比如字体、颜色等），盒模型相关的属性不可继承。
**不可继承属性**：display、margin、border、padding、background、height、min-height、max-height、width、min-width、max-width、overflow、position、top、bottom、left、right、z-index、float、clear、 table-layout、vertical-align、page-break-after、page-bread-before和unicode-bidi
**所有元素可继承属性**：visibility 和 cursor
**块级元素可继承**：text-indent 和 text-align
**内联元素可继承**：letter-spacing、word-spacing、white-space、line-height、color、font、font-family、font-size、font-style、font-variant、font-weight、text-decoration、text-transform、direction
**列表元素可继承的**：list-style、list-style-type、list-style-position、list-style-image
### @规则用法
@声明指定css要做什么，是一个通用语法。
普通@规则：
- @charset：规定浏览器使用的字符集，如`@charset "UTF-8";`
- @import：引入外部样式表
- @namespace：css的命名空间
嵌套@规则：
- @media：如果满足媒介查询的条件则条件规则组里的规则生效
- @page：描述打印文档时布局的变化
- @font-face：描述将下载的外部的字体
- @keyframes：描述 CSS 动画的中间步骤 
- @supports：support检查环境的特性，它与media比较类似
- @document：如果文档样式表满足给定条件则条件规则组里的规则生效