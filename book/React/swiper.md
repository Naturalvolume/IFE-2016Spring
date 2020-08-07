# 轮播图
用Swiper插件，实现轮播图效果
- 用useEffect，在请求到的轮播图数据长度改变 或 swiper 插件有错误时，重新初始化Swiper
- 实例化Swiper组件，第一个参数swiper类名，第二个参数设置滑动参数
```html
<div class="swiper-container">
  <div class="swiper-wrapper">
    <div class="swiper-slide">slider1</div>
    <div class="swiper-slide">slider2</div>
    <div class="swiper-slide">slider3</div>
  </div>
</div>
<script> 
var mySwiper = new Swiper('.swiper-container', {
	autoplay: 5000,//可选选项，自动滑动
})
</script>
```