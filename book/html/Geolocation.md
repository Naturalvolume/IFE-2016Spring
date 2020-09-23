# 地理定位
HTML5 `Geolocation`（地理定位）用于定位用户的位置，可能会侵犯用户隐私，所以除非用户同意，否则用户位置信息不可用。

地理定位常用来：
- 地位用户当前位置
- 更新本地信息
- 显示用户周围的兴趣点
- 交互式车载导航系统（GPS）

### navigator.geolocation.getCurrentPosition() 获得用户位置
```javascript
var x=document.getElementById("demo");
function getLocation()
{
  // 判断用户浏览器是否支持这个api
    if (navigator.geolocation)
    {
      // 执行方法，给 showPosition 方法传递位置对象 coordinates
        navigator.geolocation.getCurrentPosition(showPosition);
    }
    else
    {
        x.innerHTML="该浏览器不支持获取地理位置。";
    }
}
 
function showPosition(position)
{
  // 显示经纬度信息
    x.innerHTML="纬度: " + position.coords.latitude + 
    "<br>经度: " + position.coords.longitude;   
    
    // 可使用经纬度地图服务，如谷歌地图或百度地图
    var latlon=position.coords.latitude+","+position.coords.longitude;

    // 显示地图中对应的经纬度位置
    var img_url="http://maps.googleapis.com/maps/api/staticmap?center="
    +latlon+"&zoom=14&size=400x300&sensor=false";
    document.getElementById("mapholder").innerHTML="<img src='"+img_url+"'>";
}
}

```

### 处理错误
`getCurrentPosition()`的第二个参数用于处理错误，规定了当获取用户位置失败时运行的函数：
```javascript
function showError(error)
{
    switch(error.code) 
    {
        case error.PERMISSION_DENIED:
            x.innerHTML="用户拒绝对获取地理位置的请求。"
            break;
        case error.POSITION_UNAVAILABLE:
            x.innerHTML="位置信息是不可用的。"
            break;
        case error.TIMEOUT:
            x.innerHTML="请求用户地理位置超时。"
            break;
        case error.UNKNOWN_ERROR:
            x.innerHTML="未知错误。"
            break;
    }
}
```

### navigator.geolocation.watchPosition() 持续返回用户移动时的更新位置
对应的`navigator.geolocation.clearWatch()`用于停止`watchPosition()`方法。
