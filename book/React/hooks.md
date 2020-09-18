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
- effects不是在渲染之后发生的，是在dom运行时发生的（dom已渲染到屏幕上），要确保dom在运行effects时已经更新了；所以useEffect是在渲染时异步执行（浏览器将所有变化渲染到屏幕后才会被执行），生命周期是渲染时同步执行（和useLayoutEffect等价，都会阻塞浏览器渲染）
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

### useLayoutEffect
- useEffect 和 useLayoutEffect 的区别？

执行时机不同，useEffect是在dom渲染到屏幕上后执行的，是异步执行；useLayoutEffect是在虚拟dom更新之后执行的，此时还没有渲染，是同步执行，执行时机与 componentDidMount，componentDidUpdate 一致。
- 对于 useEffect 和 useLayoutEffect 哪一个与 componentDidMount，componentDidUpdate 的是等价的？

useLayoutEffect，从在生命周期中调用位置来看，他们都是同步调用，都会**阻塞浏览器渲染**。
- useEffect 和 useLayoutEffect 哪一个与 componentWillUnmount 的是等价的？

useLayoutEffect 的detroy函数的调用位置、时机与**componentWillUnmount**一致，都是同步调用，useEffect 的 detroy 函数从调用时机上来看，更像是 componentDidUnmount。
-  将修改dom的操作放到useEffect 和 useLayoutEffect中有什么不一样？

useEffect：异步调用，在页面渲染完成后调用，不会阻塞浏览器渲染，但修改dom后，有一次回流、重绘的代价
useLayoutEffect：同步调用，发生时浏览器处于阻塞状态，可以在最新的dom节点上修改，然后渲染，即一次渲染即可完成

### useContext
