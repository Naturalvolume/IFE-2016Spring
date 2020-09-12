# Refs
`Refs`提供了一种访问在`render`方法中创建的dom节点或者React元素的方法。
- 传统修改子组件的方法：用新的`props`重新渲染
- Refs：在典型数据流外，强制修改子代

### 方式一：Refs属性用回调函数方式使用
在组件中添加一个`ref`属性，值是一个回调函数，接收作为其第一个参数的**底层dom元素或组件的挂载实例**
```javascript
class Controllers extends React.Component {
  render() {
    <input type='text' ref={(input) => this.input = input} />
  }
}
```

### 方式二：使用React.createRef()创建Refs
通过`React.createRef()`创建refs，并通过`ref`属性附加到react元素，构造组件时，将refs分配给实例属性，可以在整个组件中引用它们
```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    this.myRef = React.createRef()
  }
  render() {
    return <div ref={this.myRef} />
  }
}
```
