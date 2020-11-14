## 通过dom元素的引用设置元素属性／样式的方法
在实际场景中，经常会有要给元素重新设置`class`或改变某个`style`来更改样式的场景，一般会通过`node.setAttribute`或`node.style.property`实现。

- `node.setAttribute`是给html元素属性设置值的，这里的属性指标签中键值对的前者，例如`<div id="name" class="text"><div>`中的`class`和`id`就是html元素属性。
- `style.property`用来设置css样式，如`document.getElementById("#name").style.background="red"`

需要注意的是，`style`也是html元素的一个属性，在设置时可以使用`document.getElementById("#name").setAttribute("style","color:red;font:9px;");`。

所以，一般用`setAttribute`更改`class`属性和`style`属性；用`style.property`更改某个具体的样式。