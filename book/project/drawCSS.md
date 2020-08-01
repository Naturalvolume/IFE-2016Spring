# 在线画板的布局
### 菜单栏
1. 利用伪类`:active`给菜单栏`tab-container`中的按钮添加点击改变颜色效果（只在点击时改变颜色）
```css
.tab-container :active {
  background-color: #b0bec5;
}
```
2. 给工具栏绑定动态样式，当`tool.ischoose`属性为`true`时，显示被选择样式（vue的对象选择样式），并且vue用数组绑定多个样式。
```html
<div :class="[{'selected':tool.ischoose},'button-item']"></div>
```
