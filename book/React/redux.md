# redux
- 与react产生关联的是`React-Redux`这个库
- redux的原理就是一个**发布－订阅器**，用一个变量存储所有的state，并提供了发布功能修改数据，以及订阅功能触发回调
- `React-Redux`的作用是订阅store里数据的更新，它包含两个重要元素，`Provider`组件和`connect`方法
- `Provider`的作用就是通过Context APi把store对象注入到react组件中
- 而connect方法就是一个高阶组件，在高阶组件里通过订阅store中数据的更新，从而通过调用setState方法触发组件更新

redux的**缺点**：
- 修改数据太麻烦，需要 dispatch(action) -> 调用reducer计算 -> 触发回调 -> 更新数据
- 或 样板代码太多，修改数据链路太长

可以通过减少模版代码来解决redux**修改数据链路太长**的痛点，简化redux api（其实就是使用封装好的语法糖）
- 使用`redux-actions`，在初始化 reducer 和 action 构造器时减少样板代码
  - 减少创建action时写一堆固定的方法：`() => ({ type: *** })` -> `createAction('***', payload => )`
  - 减少创建reducer时写一堆固定的switch：`switch{}` -> `handleActions({})`
- 使用cli工具，帮助自动生成模版代码
  - 使用`yeoman`帮助一键创建样板文件和样板代码
- redux不能处理异步，而真实业务开发中经常需要处理异步请求，比如：请求后台数据，延迟执行某个效果，`setTimeout`、`setInterval`等。
  - 可以使用中间件解决（修改dispatch方法，让它可以处理异步）

##### 为什么redux不能处理异步
```javascript
dispatch(action) // action = { type: '***', payload: '***'}

// reducer是一个纯函数，只能接收action.type来处理不同数据，无法处理其它类型的数据
// 所以dispatch接收的action不可以是其它类型

// 假设处理下面的异步请求
dispatch((dispatch) => {
  setTimeout(() => {
    dispatch({ type: 'INCREMENT'})
  }, 3000)
})
```
### react项目中引入redux的流程
主文件 index.js
```javascript
// 1. 引入redux
import { createStore, applyMiddleware } from 'redux'
import { Provider } from 'react-redux'  // react-redux 让react可以使用redux

// 2. 创建redux实例
const middleware = applyMiddleware(redux-thunk) // 创建中间件
const store = createStore(reducers, middleware) // 参数为 reducer函数 和 中间件

// 3. 注入 store 对象
const rootEl = document.getElementById('root')
const render = () => React.render(
  // 用Provider包裹要使用store的组件，注入store对象
  <Provider store={store}>
    <Component/>
  </Provider>,
  rootEl
)
```

某个组件文件 component.js
```javascript
// 1. 引入注入store
import { connect } from 'react-redux'

class Component extends React.Component {
  constructor(props) {
    super(props)
  }

  render() {

  }
}
// 2. 定义状态映射函数
const mapStateToProps = state => {
  return {

  }
}
// 3. 定义dispatch映射函数
const mapDispatchToProps = dispatch => {
  return {

  }
}
// 4. 将两个函数映射到组件中
export default connect(mapStateToProps, mapDispatchToProps)(Component)
```

### 手写redux
redux本质上是由发布－订阅模式实现的，用一个变量存储所有的State，并且提供发布功能修改数据，以及订阅功能触发回调。（在本例中，使用订阅subscribe每次重新渲染render）

```html
<html>
<head>
  <title>Document</title>
</head>
<body>
  <div>
    <p>
      Clicked: <span id="value">0</span> times
      <button id="increment">+<button>
      <button id="decrement">-<button>
      <button id="incrementIfOdd">Increment if odd<button>
    </p>
  </div>
</body>
</html>
```

```javascript
import { createStore } from 'redux'

function reducer(state, action) {
  if(typeof state === 'undefined') { return 0 }
  switch(action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}

const store = createStore(reducer)

var valueEl = document.getElementById('value')

// 渲染value
function render() {
  valueEl.innerHTML = store.getState().toString()
}

// 首次手动渲染
render()

// 订阅数据更新，就执行render方法
store.subscribe(render)

// +1 按钮
document.getElementById('increment').addEventListener('click', function() {
  store.dispatch({type: 'INCREMENT'})
})
// -1 按钮
document.getElementById('decrement').addEventListener('click', function() {
  store.dispatch({type: 'DECREMENT'})
})
// 奇数才+1
document.getElementById('incrementIfOdd').addEventListener('click', function() {
  if(store.getState() % 2 !== 0) {
    store.dispatch({type: 'INCREMENT'})
  } 
})
```

书写自己的redux库
```javascript
/**
* @param {Function} reducer
* @param {any} preloadedState 初始化的state，很少用，一般在服务端渲染的时候用
* @param {Function} enhancer 中间件
**/

export default function createStore(reducer, preloadedState, enhancer) {
  // 实现第二个形参选填
  // 只有当第二参数是中间件才会执行下面的代码
  if(typeof preloadedState === 'function' && typeof enhancer === 'undefined') { 
    enhancer = preloadedState
    preloadedState = undefined
  }
  let currentReducer = reducer
  let currentState = preloadedState   // 存储整个应用的state
  let currentListeners = [] // 订阅传进来的回调函数

  // 一个很重要的设计
  let nextListeners = currentListeners
  // 方法一：获得当前状态
  function getState() {
    return currentState
  }
  // 方法二：订阅函数，传入回调函数
  function subscribe(listener) {
    if(nextListeners === currentListeners) {
      // 进行浅复制，避免直接操作currentListener
      // 因为其它地方会用到currentListeners
      nextListeners = [...currentListeners]
    }
    nextListeners.push(listener)

    return function unsubscribe() {
      if(nextListeners === currentListeners) {
        nextListeners = [...currentListeners]
      }
      // 删除要取消的回调函数
      const index = nextListeners.indexOf(listener) // 闭包，保存listener变量
      nextListeners.splice(index, 1)
    }
  }
  /*
  这样使用 subscribe 和 unsubscribe
  const a = Store.subscribe(() => {})
  a.unsubscribe()
  */
  
  // 方法三：发布器，根据当前action修改数据，以及订阅功能触发回调
  function dispatch(action) {
    currentState = currentReducer(currentState, action) // 调用reducer更新数据
    const listeners = (currentListeners = nextListeners)  // 保证当前的listeners是最新的
    for(let i = 0; i < listeners.length; i++) {
      listeners[i]()  // 依次执行回调函数
    }

    return action
  }
  // 手动触发一次dispatch，初始化
  dispatch({type: 'INIT'})

  return {
    getState, 
    dispatch, 
    subscribe
  }
}
```

### Provider组件实现原理
在使用redux时，第三步用`<Provider>`组件把`store`注入到组件中，`<Provider>`就是通过react的`Context API`把数据往下传。

```javascript
import React from 'react'
import PropTypes from 'prop-types'

export default class Provider extends React.Component {
  // react 的三种数据：
  // context 往所有子组件、孙组件中传递数据
  // props 父组件往子组件里传递数据
  // state 组件自身的数据

  // 声明一个 context 数据，react16中有新写法
  getChildContext() {
    return { store: this.store }
  }
  constructor(props, context) {
    super(props, context)
    this.store = props.store
  }

  render() {
    return React.Children.only(this.props.children)
  }
}

Provider.childContextTypes = {
  store: PropTypes.object
}
```

### connect实现原理
connect用来映射Provider中的数据到组件中

```javascript
import React from 'react'
import PropTypes form 'prop-types'

// () => () => {} 函数柯里化
// const sum = (a) => {return (b) => a + b}
// sum(1)(2) -> connect(mapStateToProps, mapDispatchToProps)(Comp)

const connect = (mapStateToProps = state => state, mapDispatchToProps = {}) => (WrapComponent) => {   // 传入组件，返回新组件
  return class ConnectComponent extends React.Component {
    static contextTypes = {
      store: PropTypes.object
    }

    constructor(props, context) {
      super(props, context)
      this.state = {
        props: {} // 声明了一个叫做props的state
      }
    }

    componentDidMount() {
      const { store } = this.context    // 从context中拿到store对象
      store.subscribe(() => this.update())  // 订阅redux的数据更新
      this.update()
    }

    update() {
      const { store } = this.context  // 从context中拿到store对象
      const stateProps = mapStateToProps(store.getState())  // 把store中的全部数据传到组件内部
      const dispatchProps = mapDispatchToProps(store.dispatch)  // 把store.dispatch传到组件内部

      // 调用setState触发组件更新
      // 将最新的state以及dispatch合并到当前组件的props上
      this.setState({
        props: {
          ...this.state.props,
          ...stateProps,
          ...dispatchProps
        }
      })
    }

    render() {
      // 传入props
      return <WrapComponent {...this.state.props}></WrapComponent>
    }
  }
}
export default connect

/*
const mapStateToProps = state => ({
  return {
    value: state
  }
})

const mapStateToProps = dispatch => {
  return {
    onIncrement: () => dispatch({type: 'INCREMENT'}),
    onDecrement: () => dispatch({type: 'DECREMENT'})
  }
}
*/
```

`connect`就是一个高阶组件，接收`Provider`传递过来的 store 对象，并订阅store中的数据，如果store中的数据发生改变，就调用`setState`触发组件更新

