# 获取dom元素的方法
获得的dom元素有两种集合`NodeList` 和 `HTMLCollection`，分别由`querySelector`和之前的`getElement`类方法获得。
### NodeList 和 HTMLCollection
对于一个页面上的元素，`HTMLCollection`只包含`html`元素，而`NodeList`不光包含`html`，还包含`text node`(字符节点) 和 `comment`（注释节点）。

`NodeList`对象继承于`Object`对象，具有`length`属性和`entires`、`forEach`、`item`、`keys`、`values`五个方法。

`HTMLCollection`对象也继承于`Object`对象，所以它和`NodeList`是平级的，且一样包含查询得到的html元素，有`length`属性和`item`方法、`namedItem`方法(根据id和name筛选元素)。
```html
<body>
    <div class="test-div">
        div1
        <div class="test-div">
            div2
            <div class="test-div">
                div3
            </div>
        </div>
    </div>
</body>
```
1. querySelectorAll —— 返回NodeList对象

`querySelectorAll`只能取到已命名标签，隐藏的文本标签和注释标签取不到。

```javascript
console.log(document.querySelectorAll('div')) 

// NodeList(3) [div.test-div, div.test-div, div.test-div]
```

2. querySelector —— 返回node对象

`querySelector`能取到指定node对象（有很多个同名节点时，只取第一个），返回这个节点，`node`对象拥有`childNodes`属性，且当它是父节点时，拥有子元素，所以又是`ParentNode`对象（拥有`children`属性）。
```javascript
let d = document.querySelector('div')
console.log(d)
/* <div class="test-div">
        div1
        <div class="test-div">
            div2
            <div class="test-div">
                div3
            </div>
        </div>
    </div>
*/
console.log(d.childNodes)
// NodeList(3) [text, div.test-div, text]
```

`childNodes`可以返回对应dom节点下的所有dom节点，包括`text node`(用text表示，换行符也会被显示出来)。
```javascript
console.log(d.children)
// HTMLCollection [div.child]
```

`children`返回HTMLCollection对象，只包括html元素。
3. getElementsByTagName —— 返回HTMLCollection对象

```javascript
console.log(document.getElementsByTagName('div'))

// HTMLCollection(3) [div.test-div, div.test-div, div.test-div]
```

### 统计页面中标签次数的方法
方法一：document.querySelectorAll('*')
`querySelectorAll()`通过匹配选择器，获得对应的标签，返回`NodeList`对象
- 通配符选择器`*`可以用来匹配所有标签元素
- 通过标签元素的`tagName`属性可以拿到标签的名称

[如何统计页面标签使用次数？](https://www.cnblogs.com/zjmx/p/11866167.html)
