# 一些比较难的面试题

```javascript
var a = 0, b = 0
function A(a) {
  A = function(b) {
    alert(a + b++)
  }
  alert(A++)
}
A(1)
A(2)
```

闭包的优点：保存或保护