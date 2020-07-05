# 实现将url的参数转化为对象
Node.js 中有一个queryString模块，可以实现将 `urlStr = 'http://www.inode.club?name=koala&study=js&study=node'` 主机地址后面的参数转化为对象。

```javascript
function regstr(str) {
  let params = str.split('?')[1]
  params = params.split('&')
  let obj = {}
  for (let i = 0; i < params.length; i++) {
    let str = params[i].split('=')
    const key = str[0]
    const val = str[1]
    if(obj[key] === undefined) {
      obj[key] = val
    } else {
      // 判断obj[key]是不是数组，添加重复参数时是以数组形式保存的
      obj[key] = Array.isArray(obj[key]) ? obj[key] : [obj[key]]
      obj[key].push(val)
    }
  }
  return obj
}
let urlStr = 'http://www.inode.club?name=koala&study=js&study=node';
console.log(regstr(urlStr))     
```