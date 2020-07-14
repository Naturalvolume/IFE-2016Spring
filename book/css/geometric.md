# 用css画几何图形
[纯css画图形](https://blog.csdn.net/qq_39897978/article/details/90172695)
### 圆
直接用 border-radius:50% 即可
### 三角形
### 扇形
90度扇形 即让一边的圆角半径等于正方形元素的宽高

指定 css 的每个圆角有两种方法：
- border-radius：四个值（从左上角按顺时针定义） | 三个值（左上、右上＋左下、右下） ｜ 两个值（左上＋右下、右上＋左下） ｜ 一个值（全部）
- border-top-left-radius、border-top-right-radius、border-bottom-left-radius、border-bottom-right-radius
```css
    .container {
      width: 200px;
      height: 200px;
      background-color: red;
      border-top-left-radius: 100%;
    }
```
### 菱形