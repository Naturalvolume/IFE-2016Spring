## 闭包的使用场景
1. 返回值（最常用）

```javascript
//1.返回值 最常用的
function fn(){
  var name="hello";
  return function(){
      return name;
  }
}
var fnc = fn();
console.log(fnc())//hello
```
以闭包的形式将name返回

2. 函数赋值

```javascript
var fn2;
function fn(){
  var name="hello";
  //将函数赋值给fn2
  fn2 = function(){
      return name;
  }
}
fn()//要先执行进行赋值，
console.log(fn2())//执行输出fn2
```

在闭包里给fn2函数设置值，闭包的形式把name属性记忆下来，执行会输出hello

3. 函数参数

```javascript
function fn(){
  var name="hello";
  return function callback(){
      return name;
  }
}
var fn1 = fn()//执行函数将返回值（callback函数）赋值给fn1，

function fn2(f){
  //将函数作为参数传入
  console.log(f());//执行函数，并输出
}
fn2(fn1)//执行输出fn2
```

用闭包返回一个函数，把此函数作为另一个函数的参数，在另一个函数里面执行这个函数，最终输出hello

4. IIFE（自执行函数）

```javascript
(function(){
    var name="hello";
    var fn1= function(){
        return name;
    }
    //直接在自执行函数里面调用fn2，将fn1作为参数传入
    fn2(fn1);
})()
function fn2(f){
    //将函数作为参数传入
    console.log(f());//执行函数，并输出
}
```

直接在自执行函数中将封装的函数fn1传给fn2，作为参数调用同样可以获得结果hello

5. 循环赋值

```javascript
//每秒执行1次，分别输出1-10
for(var i=1;i<=10;i++){
    (function(j){
        //j来接收
        setTimeout(function(){
            console.log(j);
        },j*1000);
    })(i)//i作为实参传入
}
```

若不采用闭包，会有不一样的情况

6. getter和setter

```javascript
function fn(){
    var name='hello'
    setName=function(n){
        name = n;
    }
    getName=function(){
        return name;
    }

    //将setName，getName作为对象的属性返回
    return {
        setName:setName,
        getName:getName
    }
}
var fn1 = fn();//返回对象，属性setName和getName是两个函数
console.log(fn1.getName());//getter
fn1.setName('world');//setter修改闭包里面的name
console.log(fn1.getName());//getter
```

第一次输出hello用setter以后再输出world，这样做可以封装成公共方法，防止不想暴露的属性和函数暴露在外部

7. 迭代器（执行一次函数往下取一个值）

```javascript
var arr =['aa','bb','cc'];
function incre(arr){
    var i=0;
    return function(){
        //这个函数每次被执行都返回数组arr中 i下标对应的元素
         return arr[i++] || '数组值已经遍历完';
    }
}
var next = incre(arr);
console.log(next());//aa
console.log(next());//bb
console.log(next());//cc
console.log(next());//数组值已经遍历完
```

8. 首次区分（相同的参数，函数不会重复执行）

```javascript
var fn = (function(){
    var arr=[];//用来缓存的数组
        return function(val){
            if(arr.indexOf(val)==-1){//缓存中没有则表示需要执行
                arr.push(val);//将参数push到缓存数组中
                console.log('函数被执行了',arr);
                //这里写想要执行的函数
            }else{
                console.log('此次函数不需要执行');
            }
            console.log('函数调用完打印一下，方便查看已缓存的数组：',arr);
        }
    })();

fn(10);
fn(10);
fn(1000);
fn(200);
fn(1000);
```

9. 缓存

```javascript
//比如求和操作，如果没有缓存，每次调用都要重复计算，采用缓存已经执行过的去查找，查找到了就直接返回，不需要重新计算

var fn=(function(){
  var cache={};//缓存对象
  var calc=function(arr){//计算函数
      var sum=0;
      //求和
      for(var i=0;i<arr.length;i++){
          sum+=arr[i];
      }
      return sum;
  }

  return function(){
      var args = Array.prototype.slice.call(arguments,0);//arguments转换成数组
      var key=args.join(",");//将args用逗号连接成字符串
      var result , tSum = cache[key];
      if(tSum){//如果缓存有   
          console.log('从缓存中取：',cache)//打印方便查看
          result = tSum;
      }else{
          //重新计算，并存入缓存同时赋值给result
          result = cache[key]=calc(args);
          console.log('存入缓存：',cache)//打印方便查看
      }
      return result;
  }
})();
fn(1,2,3,4,5);
fn(1,2,3,4,5);
fn(1,2,3,4,5,6);
fn(1,2,3,4,5,8);
fn(1,2,3,4,5,6);
```

参考：[理解JavaScript闭包9大使用场景](https://mp.weixin.qq.com/s/Mk-q1WdZ8tVnxzBNiKgDrQ)

### 闭包的缺点
- 内存泄漏

```javascript
  function outer(){
  var num=0;//内部变量
  return function add(){//通过return返回add函数，就可以在outer函数外访问了
  num++;//内部函数有引用，作为add函数的一部分了
  console.log(num);
  };
 }
  var func1=outer();
  func1();//实际上是调用add函数， 输出1
  func1();//输出2 因为outer函数内部的私有作用域会一直被占用
  var func2=outer();
  func2();// 输出1  每次重新引用函数的时候，闭包是全新的。
  func2();// 输出2
```

参考：[深入浅出Javascript闭包](https://mp.weixin.qq.com/s/0zqlr0AS8SQRrocO5f_k0g)