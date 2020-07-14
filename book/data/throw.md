## 抛出自定义类型的错误对象
throw 关键字可以用来抛出错误，Error 是内置的错误类型对象。
```javascript
    function MyError(message) {
        this.message = "注意：这是自定义的错误"
        this.name = "自定义错误";
    }
    MyError.prototype = new Error();
    try {
        throw new MyError("注意：这是自定义错误类型")
    }catch (error){
        console.log(error.message)
    }
```