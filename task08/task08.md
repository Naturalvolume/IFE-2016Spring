##### 1.响应式布局中box-sizing的使用

在 [CSS 盒子模型](https://developer.mozilla.org/en-US/docs/CSS/Box_model)的默认定义里，你对一个元素所设置的 [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width) 与 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height) 只会应用到这个元素的内容区。如果这个元素有任何的 [`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border) 或 [`padding`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding) ，绘制到屏幕上时的盒子宽度和高度会加上设置的边框和内边距值。这意味着当你调整一个元素的宽度和高度时需要时刻注意到这个元素的边框和内边距。当我们实现响应式布局时，这个特点尤其烦人。

box-sizing 属性可以被用来调整这些表现:

- `content-box`  是默认值。如果你设置一个元素的宽为100px，那么这个元素的内容区会有100px 宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。

- `border-box` 告诉浏览器：你想要设置的边框和内边距的值是包含在width内的。也就是说，如果你将一个元素的width设为100px，那么这100px会包含它的border和padding，内容区的实际宽度是width减去(border + padding)的值。大多数情况下，这使得我们更容易地设定一个元素的宽高。

  三种情况如下：

  ```
  box-sizing: content-box;
  width: 100%;
  ```

![1568360663382](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1568360663382.png)

```
box-sizing: content-box;
width: 100%;
border: solid #5B6DCD 10px;
padding: 5px;
```

![1568360716138](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1568360716138.png)

```
box-sizing: border-box;
width: 100%;
border: solid #5B6DCD 10px;
padding: 5px;
```

![1568360746898](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1568360746898.png)

##### 2.仿照bootstrap的网格系统，命名DOM元素

**col-md-4**：col是“列”column的缩写；md是medium的缩写，适用于屏幕宽度大于768px的场景；4是占四栏的意思。因此，这个类的意思是，在屏幕宽度大于768px时，该元素占四栏。

**col-设备-数量**：设备类型可以为xs (phones), sm (tablets), md (desktops), and lg (larger desktops)

##### 3.

##### 4.参考：

响应式网格（栅格化）布局总结：http://www.360doc.com/content/18/0615/10/44762515_762579378.shtml

box-sizing:https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-sizing

bootstrap grid examples：https://getbootstrap.com/docs/3.4/examples/grid/

