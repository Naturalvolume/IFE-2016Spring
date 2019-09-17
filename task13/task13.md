**addEventListener()** 方法用于向指定元素添加事件句柄，使用**removeEventListener()**方法来移除 **addEventListener()** 方法添加的事件句柄。

```
document.getElementById("myDiv").addEventListener("click", myFunction, true);
```

第一个参数，表示触发事件的类型，比如click、mousemove等；

第二个参数，表示事件触发后需要执行的函数；

第三个参数，表示事件触发的阶段是冒泡还是捕获，true表示事件句柄在捕获阶段执行，false为默认参数，表示在冒泡阶段执行。

更多实例参考：[菜鸟教程](https://www.runoob.com/jsref/met-element-addeventlistener.html)

