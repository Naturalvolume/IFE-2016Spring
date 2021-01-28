### Map和Object的区别
Map和Object都可以用来存储键值对，它们的区别是：
- Map中键的范围不仅是字符串，可以是各种类型的值（包括对象）；而Object的键只能是字符串类型。

所以Object提供了“字符串－值”的对应，而Map结构提供了“值－值”但对应，是一种更完善的hash结构实现。
- Map默认情况不包含任何键，只包含显式插入的键；而Object有一个原型prototype，所以原型链上但键名可能和自己在对向上设置的键名产生冲突。

虽然es5可以用`Object.create(null)`创建一个没有原型的对象，但这种用法不常见。

- Map中的键key是有序的，因此迭代时以Map对象的插入顺序返回键值；而Object的键是无序的，以字符串的顺序排列。

- Map的键值对个数可以通过`size`属性获取，而Object的键值对个数只能通过手动循环统计。
- Map的键值对是可迭代对象iterable的，所以可以直接被迭代；而Object需要以某种方式获取它的键然后才能迭代。
- Map在频繁增删键值对的场景下表现更好，Object未对频繁增删键值对作出优化。

### Map和Object的互相转换
1. Map转为数组

用扩展运算符`...`
```javascript
const page_info = new Map();
page_info.set("title", "javascript es6的map映射");
page_info.set("author", "infoq");
console.log([...page_info]); // [ [ 'title', 'javascript es6的map映射' ], [ 'author', 'infoq' ] ]
```

2. Map转为Object

通过迭代给对象赋值
```javascript
function mapToObj(map) {
const obj = Object.create(null);
map.forEach((v,k)=>{
obj[k] = v;
});
return obj;
}
const page_info = new Map();
page_info.set("title", "javascript es6的map映射");
page_info.set("author", "infoq");
﻿
console.log( mapToObj(page_info));
```

3. 数组转为Map

直接将数组传入Map构造函数即可，即`new Map(arr)`

4. 对象转为Map

通过`Object.entries()`获得对象的键值对
```javascript
const page_info = {
title:"javascript es6的map映射",
author:"infoq"
};
console.log(new Map(Object.entries(page_info))); // Map { 'title' => 'javascript es6的map映射', 'author' => 'infoq' }
```

5. Map转为JSON

先把Map转为对象，然后使用`JSON.stringify`
```javascript
function mapToObj(map) {
const obj = Object.create(null);
map.forEach((v,k)=>{
obj[k] = v;
});
return obj;
}
function mapToJson(map){
return JSON.stringify(mapToObj(map));
}
const page_info = new Map();
page_info.set("title", "javascript es6的map映射");
page_info.set("author", "infoq");
console.log( mapToJson(page_info)); // {"title":"javascript es6的map映射","author":"infoq"}
```

参考：[ECMAScript 6 的Map映射](https://mp.weixin.qq.com/s/CjaVV14A0ME0bDm02i1Kfg)