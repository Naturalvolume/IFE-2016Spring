# context
当在多层嵌套组件中使用另一个嵌套组件提供的数据时，最简单的方法是把一个`prop`从每个组件一层层传递下去，从源组件传递到深层嵌套组件，这叫做`prop drilling`，这会让**原本不需要数据的组件变得不必要地复杂**，并且难以维护。

避免`prop drilling`的常用方法使用`React Context`，通过组件树提供了一个传递数据的方法，避免在每一个层级手动传递`props`属性，通过定义提供数据的`Provider`组件，并允许通过嵌套的组件通过`Consumer`组件或`useContext`使用上下文数据。

待看：[聊一聊我对 React Context 的理解以及应用](https://www.jianshu.com/p/eba2b76b290b)