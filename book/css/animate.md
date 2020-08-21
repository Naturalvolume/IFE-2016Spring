# css3动画
css3 动画分为两种：一种是改变元素形状的`transform`，它没有时间控制概念，一种是让元素在规定时间内缓慢变化形状或属性的`transition`和`animation`，这两个才是真正的动画。
### transform
元素变形属性，可以平移、旋转元素等
- 平移：`translate(x轴值, y轴值)`
- 旋转：`rotate(角度 deg)`  只支持一个角度，单位deg
- 缩放：`scale(水平倍数，垂直倍数)`
- 扭曲：skew
- 重设参照点: `transform-origin: X || Y || Z`，单位可以是em、px、百分比、left／right等 
`transform`还可以启用gpu加速，`will-change: transform`
### transition
`transition`是`transition-property`、`transition-duration`、`transition-timing-function`、`transition-delay`的简写形式。
它们可以取值为：
- transition-property: all
- transition-duration、transition-delay 以时间为单位，默认为 0s ，一定要加单位，否则该属性无法识别。
- `transition-timing-function: linear | ease | ease-in | ease-out | ease-in-out | cubic-bezier(n,n,n,n)`，这几个值本质上都是`cubic-bezier(n,n,n,n)`即贝塞尔曲线形式的，关于这几个参数的具体区别可以看[参数区别](https://www.cnblogs.com/Mr-liyang/p/6762998.html)

`transition`的特性：
- display 属性不能作为变化的属性`transition-property`
- 需要用事件触发（比如用 :hover 伪类），不会在加载网页时自动触发
- 每次触发只执行一次动画
- 只能从一个状态变化到另一个状态
- 一条transition规则只能定义一个属性
- transition 完成后触发 transitionend 事件
### animation
animation 弥补了 transition 不能自动触发、不能重复触发、只有两个状态，只能定义一个属性的不足，它是以下几个属性的简写：
- `animation-name: keyframes-name | none`，不能是属性名称
- `animation-duration` 跟 transition 一致
- `animation-timing-function`跟 transition 一致
- `animation-delay`跟 transition 一致
- `animation-fill-mode(动画结束后，属性会保留在哪个状态): none(动画没开始时) | forwards(结束) | backwards(第一帧) | both`
- `animation-direction(动画播放方向): normal(正向)/alternate(交替慎用) | reverse(反向) | alternate-reverse(反向交替慎用)`
- `animation-iteration-count: 3(播放次数) | infinite(无限)`
- `steps(10)`函数实现分步过渡
还有一个不能简写的让动画保持突然终止状态时的状态 `animation-play-state: running(例如悬停时播放) | paused(非悬停时暂停)`

animation 需要跟 keyframe 配合使用，在 keyframe 中定义动画的各个状态：
- 可以用`from`或 0% 表示开始的状态，`to`或 100% 表示结束的状态
- 可以同时定义多个属性
```css
@keyframes type {
  from { left: 0; top: 0; }
  to { left: 100%; top: 100%; }
}

@keyframes type {
  from { left: 0; top: 0; }
  50% {left: 50%; top: 50%;}
  to { left: 100%; top: 100%; }
}
```
`animation`的注意事项：
- `animation`声明的 CSS 属性优先级最高，在实现中是比 `!important`要高，但标准文档中的说法应该是可以被 !important 声明的属性所覆盖
- 可以用 animation-name 指定多个动画效果，所以当所有动画会修改相同的属性时，后指定的动画会覆盖掉前面的
```javascript
#colors {
  animation-name: red, green, blue; /* 假设这些 keyframe 都是修改 color 这个属性 */
  animation-duration: 5s, 4s, 3s;
}
```
上述代码的动画效果会是这样：前 3 秒是 blue，然后接着 1 秒是 green，最后 1 秒是 red。整个覆盖的规则是比较简单的。
- 若有元素的 display 设置为 none，那么在它或者它的子元素上的动画效果便会**停止**，而重新设置 display 为可见后，动画效果会重新**重头开始执行**。
- 监听 animation 的事件：animationstart 动画开始事件（在delay之后）、animationend 动画结束的事件（多个动画会触发多次）、animationiteration 无限循环时每结束一次就触发（不是整个动画结束时触发）

### css动画的性能
能开启gpu加速的属性`transform`和`opacity`，所以放大字体时，用`transform: scale()`比`font-size`性能更好，还有`will-change`
### 题目
让div元素1s后变大，2s后变圆
```css
@keyframes change {
  50% { 
    width: 100px;
    height: 200px;
  }
  100% { 
    border-radius: 50%;
  }
}
animation: change 2 linear;
```