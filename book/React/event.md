# React事件
`SyntheticEvent`是react跨浏览器的原生事件包装器，解决了跨浏览器兼容性问题，有和浏览器原生事件相同的接口，包括`stopPropagation()`和`preventDefault()`。

react 不把事件附加到子节点本身，而是使用单个事件侦听器侦听顶层的所有事件，对性能有好处，意味着react在更新dom时不需要跟踪事件监听器