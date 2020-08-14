```javascript
class MyPromise {
  // executor是实例化promise时传入的函数，是 (res, rej) => {} 形式，res、rej都是函数
  constructor (executor) {
    this.status = 'pending'
    this.successVal = null
    this.failVal = null
    // 用数组存储回调函数，当重复调用then时
    this.resolvedCallback = []
    this.rejectedCallback = []
    // 不能直接写成 resolve(value) {} 的形式定义resolve函数
    const resolved = (value) => {
      console.log("resolved")
      // promise状态只能被改变一次
      if(this.status !== 'pending') return
      // 用setTimeout实现promise的异步调用效果，当然浏览器底层并不是用它实现的
      // 只是js并没有接触到底层的东西，所以无法模拟
      setTimeout(() => {
        this.successVal = value
        this.status = 'fulfilled'
        this.resolvedCallback.forEach(fn => fn(this.successVal))
      }, 100)
    }
    const rejected = (error) => {
      if(this.status !== 'pending') return
      setTimeout(() => {
        this.failVal = error
        this.status = 'rejected'
        this.rejectedCallback.forEach(fn => fn(this.failVal))
      }, 0)      
    }
    // 执行传入函数
    try {
      executor(resolved, rejected)
      // 初始化错误，捕捉错误
    } catch (e) {
      rejected(e)
    }   
  }
  // then也是两个回调函数参数
  then(onResolved, onRejected) {
    console.log(this.status)
    // 判断参数是否确实为函数
    // 若不是函数，继续向下传递 resolve 的值，称为返回值穿透
    onResolved = typeof onResolved === 'function' ? onResolved : value => value
    // 若失败回调不是函数，或者没有传入失败回调，就继续向下抛出错误
    // 注意 throw error要加大括号，不加大括号 代表 return throw error 是错误的语法
    onRejected = typeof onRejected === 'function' ? onRejected : error => {throw error}

    let resPromise

    if(this.status === 'pending') {
      resPromise = new MyPromise((res, rej) => {
        this.resolvedCallback.push((value) => {
          try {
            // 做实验，失败的时候，还会有成功回调存储吗
            let data = onResolved(value)
            res(data)
          } catch (e) {
            rej(e)
          }
          
        })
        this.rejectedCallback.push((error) => {
          let data = onRejected(error)
          rej(data)
        })
      })
    } else if(this.status === 'fulfilled') {
      resPromise = new MyPromise((res, rej) => {
        setTimeout(() => {
          try {
            let data = onResolved(this.successVal)
            res(data)
          } catch (e) {
            rej(e)
          }
          
        }, 0)
      })
    } else if (this.status === 'rejected') {
      resPromise = new MyPromise((res, rej) => {
        setTimeout(() => {
          try {
            // 失败回调执行完，还能继续向下传递值
            let data = onRejected(this.failVal)
            res(data)
          } catch (e) {
            rej(e)
          }
        })
      })
    }
    return resPromise
  }
  // need think think
  catch (onRejected) {
    return this.then(null, onRejected)
  }
  
  resolvePromise(resPromise, data, res, rej) {
    if(resPromise === data) return rej(new TypeError("循环引用"))
    if(!(resPromise instanceof MyPromise)) return res(data)

    try {
      const resloveFunction = (newData) => {
        this.resolvePromise(resPromise, newData, res, rej)
      }
      const rejectFunction = (error) => {
        rej(error)
      }
      data.then(resloveFunction, rejectFunction)
    } catch (error) {
      rej(error)
    }
  }

  // resolve 可以有三种值
  static resolve(val) {
    // MyPromise 对象
    if(val instanceof MyPromise) return val
    // thenale对象，即{then: ()=>{resolve(value)}}有then函数的对象，执行then中的resolve
    // 改变状态
    return new MyPromise((res, rej) => {
      if(val && val.then && typeof val.then === 'function') {
        val.then(res, rej)
      } else {
        res(val)
      }   
    }) 
  }
  static reject(error) {
    return new MyPromise((res, rej) => {
      rej(error)
    })
  }
  static all(promiseArr) {
    return new MyPromise((res, rej) => {
      if(!Array.isArray(promiseArr)) return rej(new TypeError("传入类型必须为数组"))
      // 保存结果数组
      let resArr = new Array(promiseArr.length)
      // 保存执行成功的promise数量
      let count = 0
      for(let i = 0; i < promiseArr.length; i++) {
        // 不需要判断是否为promise类型，因为all的参数可以不是promise的
        // if(!(promiseArr[i] instanceof MyPromise)) return rej(new TypeError(""))
        // promise数组中 可能不是promise对象，所以用 resolve 再包裹一下
        MyPromise.resolve(promiseArr[i]).then((item) => {
          resArr[i] = item
          count++
          // 最后一个promise执行完，返回promise数组
          if(count === promiseArr.length) return res(resArr)
        }).catch(err => rej(err))
      }
    })
  }

  static race(promiseArr) {
    return MyPromise((res, rej) => {
      if(!Array.isArray(promiseArr)) return rej(new TypeError("promiseArr must be an array"))
      for(let i = 0; i < promiseArr.length; i++) {
        // 只要有一个执行完成了，就返回
        MyPromise.resolve(promiseArr[i]).then((data) => {
          res(data)
          return
        }).catch(err => {
          rej(err)
          return
        })
      }
    })
  }
  // 不管状态如何，都会调用finally中的方法
  finally(callback) {
    this.then(value => {
      return MyPromise.resolve(callback()).then(() => {
        return value
      })
    }, error => {
      return MyPromise.resolve(callback()).then(() => {
        throw error
      })
    })
  }
}
```