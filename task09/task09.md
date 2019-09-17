#### 1.表单中placeholder属性

```
 <input type="text" class="search" placeholder="搜索">
```

![img](https://box.kancloud.cn/471614f7ad881782023185a23a0eaf0c_483x139.png)

#### 2.父元素中的所有子元素都是position=absolute布局时，由于绝对布局的元素不在整个元素流中，因此所有子元素流出，造成父元素高度为0，导致设置的背景色不显示。

#### 3.命名规范

```
HTML文件中<style>和<javascript>标签中的元素与其对齐，不需添加空格。

<style type="text/css">
nav{
	background-color: #0EBEBE;
}
</style>
```

外部引用CSS文件时，CSS文件首位不需添加style标签。

#### 4.定义字体颜色直接使用color属性

没有font-color属性

#### 5.行间距line-height属性对于块级元素和行内元素的使用效果不同

