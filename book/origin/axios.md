# 用promise实现ajax请求
```javascript
function getAjax(url) {
  return new Promise((resolve, reject) => {
    let xhr = new XMLHttpRequest()
    // 配置参数
    xhr.open('GET', url)
    xhr.send()
    xhr.onreadystatechange = function() {
      if(xhr.readyState===4) {
        if(xhr.status===200){
          resolve(JSON.parse(xhr.responseText))
        }else{
          reject('ERROR')
        }
      }   
    } 
  })
}
```