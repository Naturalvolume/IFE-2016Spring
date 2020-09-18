# js原生dom操作API
### Node 
很多类型的dom元素都继承于它，共享着相同的基本属性和方法。常见的node有 element、text、attribute、comment、document等，node的属性`nodeType`表示node的类型，是一个整数。
```javascript
{
    ELEMENT_NODE: 1, // 元素节点
    ATTRIBUTE_NODE: 2, // 属性节点
    TEXT_NODE: 3, // 文本节点
    DATA_SECTION_NODE: 4,
    ENTITY_REFERENCE_NODE: 5,
    ENTITY_NODE: 6,
    PROCESSING_INSTRUCTION_NODE: 7,
    COMMENT_NODE: 8, // 注释节点
    DOCUMENT_NODE: 9, // 文档
    DOCUMENT_TYPE_NODE: 10,
    DOCUMENT_FRAGMENT_NODE: 11, // 文档碎片
    NOTATION_NODE: 12,
    DOCUMENT_POSITION_DISCONNECTED: 1,
    DOCUMENT_POSITION_PRECEDING: 2,
    DOCUMENT_POSITION_FOLLOWING: 4,
    DOCUMENT_POSITION_CONTAINS: 8,
    DOCUMENT_POSITION_CONTAINED_BY: 16,
    DOCUMENT_POSITION_IMPLEMENTATION_SPECIFIC: 32
}
```

### 节点查找API
- `document.getElementById`: 实时
- `document.getElementByClassName`: 实时
- `document.getElementByTagName`: 实时
- `document.getElementByName`: 实时
- `document.querySelector`： 非实时，固定的
- `document.querySelectorAll`： 非实时，固定的

### 节点创建API
- `createElement`
- `createTextNode`
- `createNode(bool)`: 克隆节点，用布尔参数表示是否复制子元素
- `createDocumentFragment`: 创建文档碎片，轻量级文档，存储临时节点，大量操作dom时用它可以提升性能
- `createTextNode`
- `createTextNode`

### 节点修改API
- `parent.appendChild(child)`
- `parent.insertBefore(newNode, refNode)`
- `parent.removeChild(child)`: 删除指定子节点并返回子节点
- `parent.replaceChild(newNode, oldNode)`

还有用来获取dom节点关系的API，这里就不写了

参考：[JavaScript原生DOM操作API总结](https://www.cnblogs.com/libin-1/p/6088007.html)

