# 实现instanceof
首先，明确一点，**instanceof左侧必须是对象**，才能找到它的原型链；右侧必须是**函数，函数才会有prototype属性**

```javascript
function instance_of(obj, funcObj){    
    // 验证如果为基本数据类型，就直接返回false
    const baseType = ['string', 'number','boolean','undefined','symbol']
    if(baseType.includes(typeof(obj))) { return false }
    
    while(true){           // 无线循环的写法（也可以使 for(;;) ）
        if(obj.__proto__ === null){    //找到最顶层
            return false;
        } else if(obj.__proto__ === funcObj.prototype){       //严格相等
            return true;
        }
    }
}
```