# javascript 底层数据结构
### 内置类型
javascript 目前有八种数据类型：number string boolean null undefined symbol Bigint object。 
- number 数据表示的是双精度64位浮点数，能表示的范围为-9007199254740991(-(2^53-1))和9007199254740991（(2^53-1)），所以超过此范围的会失去精度。

```javascript
console.log(999999999999999);  //=>10000000000000000
console.log(9007199254740992 === 9007199254740993;)    // → true 居然是true!
```
- Bigint（es10）可以表示超过 number 范围的数，克服大数据的精度损失问题，使用Bigint只需要在数字末尾追加`n`即可或使用构造函数`new Bigint`，具体可看[谈谈对bigint对理解](http://47.98.159.95/my_blog/js-base/007.html#%E4%BB%80%E4%B9%88%E6%98%AFbigint)

在力扣上有一道题就用到了Bigint:[剑指 Offer 14- II. 剪绳子 II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)
```javascript
var cuttingRope = function(n) {
    if(n<=2) return 1
    if(n==3) return 2
    // 所有和 BigInt 数据进行运算的数字都要是BigInt数据类型
    let num = BigInt(n)
    // BigInt数据的除法会自动四舍五入，不能用Math.floor
    let count = num/3n
    let residual = num%3n
    let max

    switch(residual) {
        case 0n:
          // 指数也不能用Math.pow，用两个乘法运算符代替
            max = 3n ** count
            break
        case 1n:
            max = 3n ** (count-1n) * 2n * 2n
            break
        case 2n:
            max = 3n ** count * 2n
            break
    }
    return max%1000000007n
};
```
- Symbol克服了对象属性重叠的情况，具体看[javascript变量和基本类型](https://juejin.im/post/5cc94723f265da034c7036e6)

### null 和 undefined 的不同
`null`是**空对象指针**
- 作为函数参数，表示该函数的参数不是对象
- 作为原型链的终点
- 清除闭包
```javascript
let obj = {}; // 开辟一块堆内存，里面内容是空的，有16进制的地址AAAFFF000
obj = null; // 把变量obj指向空对象指针，AAAFFF000这个堆内存会成为垃圾等待回收，但不会立即回收
// 加速对象回收
```


`undefined`是变量已定义但未赋值。
- 调用函数时，应该提供的参数没有提供，该参数等于undefined
- 对象没有赋值的属性，该属性的值为undefined
- 函数没有返回值时，默认返回undefined

### 实现一个symbol





### typeof null === 'object'
`typeof`可以准确输出所有基本数据的类型，除了`null`，这是因为javascript数据在底层都是以**32位二进制**存储的，出于性能考虑用低位存储变量的类型信息，如前三位为0的会被识别成`object`，而`null`的二进制是`00..0000`，就会被识别成`object`，这是js底层的遗留问题。
### 基本包装类型
**基本包装类型**是基本类型`string`、`number`、`boolean`，它们在一定条件下能从基本类型转换成对象，也就是原始类型的**包装对象**，即`Number``String``Boolean`，这样可以让对象涵盖所有类型的值，并且让基本类型可以使用对象的方法，如`length`等。

基本类型可以被当作包装对象使用，如调用`toString`和`valueOf`，javascript 引擎在运行时会自动把**基本类型的值**转换成**包装对象实例**，使用完后立即销毁，这就是`num = 1`和`Number(1)`的区别。

**拆箱操作**：从包装对象转成基本类型，用 valueOf 或 toString

**装箱操作**：从基本类型转成包装对象，分为 隐式装箱 和 显式装箱。

```javascript
// 隐式装箱
  let a = 'sun'
	let b = a.indexof('s') // 0 // 返回下标
	// 上面代码在后台实际的步骤为：
	let a = new String('sun')
	let b = a.indexof('s')
  a = null

// 显式装箱
let a = new String('sun	')
```

**注意**：包装对象是只读的，不能修改值
### javascript 底层存储结构
常见的数据存储类型有：数组、队列、链表、栈、堆、树、图、散列表，在javascript中使用的是堆和栈：
- 基本数据类型按值直接存储在栈中，这样每种类型的大小是确定的，并且由系统自动分配和释放，优势有：内存可以及时得到回收、比堆更方便管理内存等；
- 引用数据类型的地址用栈存储，数据用堆存储，`new`出来的布尔、数字也存储在堆中。

### 对象的底层实现
散列表／哈西表
更底层的可看：[javascript对象底层原理](https://www.cnblogs.com/full-stack-engineer/p/9684072.html)
### 0.1 + 0.2 != 0.3
0.1 和 0.2 被转换成二进制时会无限循环，由于位数限制，后面多余的位数会被截掉，此时会有精度损失，所以相加后再转换成十进制会编程 0.30000000000000004。

### 处理精度损失的方法
- 用 Bigint
- 用字符串