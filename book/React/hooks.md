# hooks + function组件
hooks是一种函数，允许从函数式组件 勾住 react状态和生命周期功能，在类内部不起作用。

hooks规则：
- 只能在顶层调用hook，不能在循环、条件或嵌套函数中调用hook
- 只能从react函数式组件中调hook
### useState
用解构语法向函数组件中添加状态，如`const [count setCount] = useState(0)`，返回当前状态值`count`和更新它的函数`setCount`，类似于类中的`this.setState`，**且在函数式组件中不需要使用this取值**
### useEffect
副作用指函数或表达式的行为（react渲染阶段）依赖于外部世界，数据获取、设置订阅以及手动更改react组件中的dom都是副作用的例子。

代替生命周期componentDidMount，componentDidUpdate 和 componentWillUnmount，即发生在每次渲染之后的方法，且返回(return)卸载时要触发的函数。

与生命周期的不同：
- effects不是在渲染之后发生的，是在dom运行时发生的，因为要确保dom在运行effects时已经更新了
- 每次重新渲染时，让不同的effect替换前一个，即在下次运行effect之前清理之前渲染的effect（从某种程度上说effect更像是渲染结果的一部分）
- 使用useEffect调用的effects不会阻止浏览器更新屏幕，这让应用程序更有响应性

使用：
- 没有依赖：每次组件渲染到屏幕后都执行
- 依赖为空数组 []：只在初次渲染阶段执行，即componentDidMount
- 添加依赖跳过effects，优化性能：每次重新渲染时对比两次的值
- 添加返回函数return：清除数据，防止内存泄露

### hook规则
- 只在最顶层使用hook，不在循环、条件或嵌套函数中调用，这是为了保证hook在每一次渲染中都按照同样的顺序调用（因为react是靠hook调用的顺序来知道state间的对应关系）
- 只在react函数中调用hook