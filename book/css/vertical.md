# 垂直方向的布局
页面分为上、中、下三部分，上、下部分高度固定，中间部分高度不固定。当页面高度小于浏览器高度时，下部分固定在屏幕底部；当页面高度超出浏览器高度时，下部分随中间部分被撑开，显示在页面最底部。

也就是要实现下部分黏贴在屏幕底部，可以使用flex 或 grid实现。

```html
<head>
<style type="text/css">
  body {
    height: 1000px;
  }
  .container {
    display: flex;
    flex-direction: column;
    height: 100%;  
  }
  header, footer {
    min-height: 100px;
    background-color: black; 
  }
  main {
    flex: 1;
  }
</style>
</head>
<body>
<div class="container">
  <header></header>
  <main></main>
  <footer></footer>
</div>
</body>
```