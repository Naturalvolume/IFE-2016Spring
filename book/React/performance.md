### 图片懒加载
react-lazyload 库实现
- 引入`import LazyLoad from "react-lazyload";`
- 用 LazyLoad 包裹img标签，在placeholder中引入占位图片，实现视口内显示真实资源，视口外显示占位图片

```html
<LazyLoad placeholder={<img width="100%" height="100%" src={require ('./music.png')} alt="music"/>}>
  <img src={item.picUrl + "?param=300x300"} width="100%" height="100%" alt="music"/>
</LazyLoad>
```

- 用forceCheck方法实现滑动的时候让下面的图片显示

```html
// 引入 forceCheck 方法
import { forceCheck } from 'react-lazyload';

//scroll 组件中应用这个方法
<Scroll className="list" onScroll={forceCheck}>
```

### 用redux进行数据缓存
```javascript
//Recommend/index.js
useEffect (() => {
  // 如果页面有数据，则不发请求
  //immutable 数据结构中长度属性 size
  if (!bannerList.size) {
    getBannerDataDispatch ();
  }
  if (!recommendList.size){
    getRecommendListDataDispatch ();
  }
}, []);
```