1.圣杯布局的三种方式：

（1）float：三个元素均要设置为float：left，且按顺序排列，然后左侧栏设置margin-left：-100%，右侧栏设置margin-left：-宽度，当右侧栏左边距设置不合理时，会出现右侧栏上不去的情况。

（2）position

（3）flex

个人认为好用的顺序为flex布局>position>float

2.CSS代码规范：

（1）包含多个selector时，每个选择器声明必须独占一行。

```
.post,
.page,
.comment {
    line-height: 1.5;
}
```

（2）在可以使用缩写的情况下，尽量使用属性缩写。

（3）同一 rule set 下的属性在书写时，应按功能进行分组，并以 **Formatting Model（布局方式、位置） > Box Model（尺寸） > Typographic（文本相关） > Visual（视觉效果）** 的顺序书写，以提高代码的可读性。

- Formatting Model 相关属性包括：`position` / `top` / `right` / `bottom` / `left` / `float` / `display` / `overflow` 等

- Box Model 相关属性包括：`border` / `margin` / `padding` / `width` / `height` 等

- Typographic 相关属性包括：`font` / `line-height` / `text-align` / `word-wrap` 等

- Visual 相关属性包括：`background` / `color` / `transition` / `list-style` 等

  ```
  .sidebar {
      /* formatting model: positioning schemes / offsets / z-indexes / display / ...  */
      position: absolute;
      top: 50px;
      left: 0;
      overflow-x: hidden;
  
      /* box model: sizes / margins / paddings / borders / ...  */
      width: 200px;
      padding: 5px;
      border: 1px solid #ddd;
  
      /* typographic: font / aligns / text styles / ... */
      font-size: 14px;
      line-height: 20px;
  
      /* visual: colors / shadows / gradients / ... */
      background: #f5f5f5;
      color: #333;
      -webkit-transition: color 1s;
         -moz-transition: color 1s;
              transition: color 1s;
  }
  ```

-  RGB颜色值必须使用十六进制记号形式 #rrggbb。不允许使用 rgb()。

- 颜色值可以缩写时，必须使用缩写形式。

  ```
  /* good */
  .success {
      background-color: #aca;
  }
  
  /* bad */
  .success {
      background-color: #aaccaa;
  }
  ```

- 颜色值不允许使用命名色值

  ```
  /* good */
  .success {
      color: #90ee90;
  }
  
  /* bad */
  .success {
      color: lightgreen;
  }
  ```

- 颜色值中的英文字符采用小写，如不小写也要保证同一项目内保持大小写一致

- 为了防止在webkit内核的浏览器中不能使用某些样式，需要加-webkit在样式前面。

  ```
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  ```

  

