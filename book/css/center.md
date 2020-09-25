# 居中方案
### 水平居中
- 行内元素
  - `text-align: center;`
- 块级元素
  - 确定宽度时
    - `margin: 0 auto;`
    - 绝对定位 + `left: 50%;` + `margin-left: -(计算得到的自身宽度的一半)`
  - 未知宽度时
    - `display: table;` `margin: 0 auto;`(在子元素上设置)
    - `display: inline-block;` `text-align: center;`
    - `display: flex;` `justify-content: center;`(在父元素上设置)
    - 绝对定位 + `left: 50%;` + `transform: translateX(-50%);`

### 垂直居中
- 行内纯文本元素
  - `line-height` 等于 `height`
- 内联元素 及 display值为table-cell
  - `vertical-align: middle;`

### 水平垂直居中
- 方法一：`flex`定位

```css
/* 父元素 */
display: flex;

/* 子元素 */
margin: auto;
```
- 方法二：`display: flex;` + `justify-content: center;`(水平) + `align-items: center;`(垂直)
- 方法三：绝对定位
  - `margin: auto;`（子元素必须有宽高，否则会自动吸收全部剩余宽度）

```css
/* 父元素 */
position: relative;

/* 子元素 */
position: absolute;
left: 0;
right: 0;
top: 0;
bottom: 0;
margin: auto;
```

  - `transform: translateX(-50%)`（兼容性不好）

```css
/* 父元素 */
position: relative;

/* 子元素 */
position: absolute;
left: 50%;
top: 50%;
transform: translate(-50%, -50%);
```

  - 已知自身尺寸，计算左`margin`和上`margin`为自身的一半

```css
/* 父元素 */
position: relative;

/* 子元素 */
position: absolute;
/* 已知宽高 */
width: 100px;
height: 100px;
left: 50%;
top: 50%;
margin-left: -50px; 
margin-top: -50px; 
```

- 方法四：`table-cell`（行内元素或设置`display: inline-block;`）

父元素要有固定宽高才可以实现（百分比不是固定宽高）
```css
.parent {
  width: 300px;
  height: 300px;
  display: table-cell;
  vertical-align: middle;
  text-align: center;
}
.son {
  display: inline-block;
}
```

- 方法四：js实现

通过获取父元素和子元素宽高，设置子元素距上边和左边的距离
```javascript
let parent = document.getElementById('parent'),
    parentW = parent.clientWidth,
    parentH = parent.clientHeight,
    son = document.getElementById('son')
    sonW = son.offsetWidth,
    sonH = son.offsetHeight

son.style.position = 'absolute'
son.style.left = (parentW - sonW)/2 + 'px'
son.style.top = (parentH - sonH)/2 + 'px'
```