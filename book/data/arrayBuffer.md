# ArrayBuffer 二进制数组
`ArrayBuffer`对象、`TypeArray`视图、`DataView`视图是js操作二进制数据的一个接口。

`WebGL`指浏览器和显卡之间的通信接口，为了满足js与显卡之间**大量的、实时的数据交换**，它们之间的数据通信最好是**二进制的**，而不是传统的文本格式。文本格式传递一个32位整数，两端的js脚本与显卡都要进行格式转换，将会非常耗时，所以`ArrayBuffer`可以像c语言一样，以数组下标的形式直接操作字节，将4个字节的32位整数以二进制形式原封不动送入显卡，提供脚本性能。

二进制数组由三类对象组成：
- `ArrayBuffer`对象：代表内存中的一段二进制数据，可以通过“视图”进行操作。“视图”部署了数组接口，可以用数组的方法操作内存
- `TypeArray`视图：包括9种类型的视图，比如`Uint8Array`（无符号8位整数）数组视图、`Int16Array`(16位整数)、`Float32Array`(32位浮点数)等
- `DataView`视图：自定义复合格式的视图，比如第一个字节是`Uint8`、第二三个字节是`Int16`、第四个字节开始是`Float32`等等，还可以自定义字节序

简单说就是，`ArrayBuffer`对象代表原始的二进制数据，`TypeArray`视图用来读写简单类型的二进制数据，`DataView`视图用来读写复杂类型的二进制数据。
因为，`ArrayBuffer`对象代表存储二进制数据的一段内存，不能直接读写，只能通过视图读写，视图的作用是**以指定格式解读二进制数据**。


二进制数组并不是真正的数组，而是类似数组的对象，很多浏览器操作的API，用到了二进制数组操作二进制数据：
- canvas
- fetch 
- file 
- WebSockets
- XMLHttpRequest

### 创建二进制数组
1.DataView视图创建

```javascript
// 创建生成一段32字节的内存区域，每个字节的值默认都为0
const buf = new ArrayBuffer(32)
// 以ArrayBuffer对象实例为参数创建视图
const dataView = new DataView(buf)
// 以不带符号的8位整数格式 从头读取8位二进制数据，结果得到0
dataView.getUint8(0)    // 0
```

2.TypedArray视图创建
TypedArray 是一组构造函数，代表不同的数据格式
```javascript
const buffer = new ArrayBuffer(12);

const x1 = new Int32Array(buffer);
x1[0] = 1;
const x2 = new Uint8Array(buffer);
x2[0]  = 2;
// 上面分别在同一块内存区域创建了两种视图
// 所以在一个视图修改底层内存，会影响到另一个视图
x1[0] // 2
```

### ArrayBuffer的属性和方法
- ArrayBuffer.prototype.byteLength: 返回所分配内存区域的字节长度
- ArrayBuffer.prototype.slice: 类似数组的slice方法，可以将内存区域的一部分，拷贝生成一个新的`ArrayBuffer`对象
- ArrayBuffer.isView(): 用来判断参数是否为`ArrayBuffer`的视图实例

### 普通数组和TypeArray数组的差异
- TypeArray 的所有成员，都是同一种类型
- TypeArray 的成员是连续的，没有空位
- TypeArray 成员默认为0，因为没有空位
- TypeArray 只是一层视图，本身不存储数据，所以要获取底层对象必须使用`buffer`属性

- 普通数组的操作方法和属性，对TypeArray数组完全适用，但是没有`concat`方法


