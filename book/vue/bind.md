# vue双向数据绑定原理

刚开始只是把vue练熟，后来去研究了底层原理，才知道双向绑定是如何实现的。项目中现在用的全是2.0，还没用到3.0，在github上看到了3.0版本，了解到有哪些改进，又在掘金、简书等博客上看到大神们的解析，
### 简单版实现
```html
<html>
<head>
<body>
  姓名：<span id = 'spanName'></span>
  <br>
  <input type='text' id='inpName'>

  <script>
  let obj = {
    name: ''
  }
  // 深拷贝对象obj，用来作为get操作的数据
  let newObj = JSON.parse(JSON.stringify(obj))
  Object.defineProperty(obj, 'name', {
    get() {
      return newObj.name
    },
    set(value) {
      // 判断是否更改，减少次数
      if(value === newObj.name) return
      newObj.name = value
      // 数据改变，更新视图
      observer()
    }
  })

  // 竟然可以直接通过 id 获取到dom元素
  function observer() {
    spanName.innerHTML = obj.name
    inpName.value = obj.name
  }
  // 数据流一：数据改变更新视图
  setTimeout(() => {
    obj.name = 'kathy'
  }, 1000)
  // 数据流二：视图改变更新数据
  inpName.oninput = function() {
    // this 就是input事件
    obj.name = this.value
  }
  </script>
</body>
</head>
</html>
```

存在的问题：
- 要克隆一份数据
- 要分别给对象中的每一个属性设置监听
- 后来添加的属性，没有被监听

**知识点**：可以直接用`id`获取dom元素，因为当`id`不和js内置属性 或 全局变量重名的话，该名称自动称为window对象的属性，指向表示文档元素的`HTMLElement`元素，所以可以直接用来操作dom。（各大浏览器虽然都支持，但还未形成标准）
