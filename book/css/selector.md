# css3新增选择器
### 属性选择器
- E[attribute]: 选择带有指定属性的元素
- E[attribute=value]
- E[attr^=“val”]: 属性值以 val 开头
- E[attr$=“val”]: 以 val 结尾
- E[attr*=“val”]: 包含 val

### 结构伪类选择器
通过文档结构的先后关系匹配特定的元素，减少结构代码中id属性和class属性的定义，使文档更加简洁。

- root: 文档根元素
- E:nth-child(n): 第n个子元素E
- E:nth-last-child(n)	: 倒数第n个
- E:nth-of-type(n)
- E:nth-last-of-type(n)
- E:last-child
- E:fisrt-child
- E:first-of-type
- E:last-of-type
- E:only-child
- E:only-of-type
- E:empty

n的取值有：
- 从1开始
- odd奇数、even偶数
- 2n表示奇数，2n+1表示偶数

### 伪类选择器
- E:enabled
- E:disabled
- E:checked

### 伪元素选择器
- ::first-letter：第一个字
- ::first-line：第一行
- ::selection：被选中的字段
- ::before 和 ::after：必须有`content`属性