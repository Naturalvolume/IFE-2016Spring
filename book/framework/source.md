# Source Map
js 脚本比较复杂，大部分源码（尤其是各种函数库和框架）都要经过转换，才能投入生产环境：
- 压缩、减小体积
- 多个文件合并，减少http请求数
- 将其它语言编译成js

所以实际运行的代码不同于开发代码，debug 比较困难，因为无法定位出错代码位置。

Source Map: 一个信息文件，存储位置信息，即转换后的代码位置 和 转换前代码的位置，所以出错后，将直接显示原始代码，而不是转换后的代码。

### mappings
mappings 是 source map 文件对象的一个属性，存储着位置信息，表示两个文件的各个位置是如何一一对应的。
- 是一个很长的字符串，分为三层
  - 第一层是**行对应**，以`;`表示，每个分号对应转换后源码的一行，所以第一个分号前内容就对应源码的第一行，以此类推
  - 第二层是**位置对应**，以`,`表示，每个逗号对应转换后源码的一个位置。所以，第一个逗号前的内容，对应源码的第一个位置
  - 第三层是位置转换，以`VLQ编码`表示，代表该位置对应的转换前的源码位置

### 位置转换
第三层使用五位，表示五个字段
- 第一位：在转换后代码的第几列
- 第二位：属于sources属性中的哪一个文件
- 第三位：转换前代码的第几行
- 第四位：转换前代码的第几列
- 第五位：属于names属性中的哪一个变量

### VLQ编码
可以非常精简的表示很大的数值

参考：[JavaScript Source Map 详解](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html)