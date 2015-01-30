# 三种实现Retina屏的1px
1.svg方法

html代码：
```html
<div class="wrap">
    <h2>左边框</h2>
    <div class="box box1"></div>
</div>
```
css代码：
```css
    .box1 { border-left: 0; background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1' height='100%'><rect fill='#c8c8c8' x='0' y='0' width='0.5' height='100%'/></svg>"); background-position: 0 0; background-repeat: no-repeat; }
```
[svg方法](https://github.com/gaostore/codes/blob/master/svg/demo.html)

2.scale方式：通过缩放实现

html代码：
```html
<div class="wrap">
    <h2>使用scale方法实现</h2>
    <div class="box"></div>
</div>
```
css代码：
```css
.box { position: relative; padding: 1px; border: 0 none; margin: 0 auto; height: 200px; width: 200px; background: #fff; }
.box:before { content: ''; position: absolute; top: 0; left: 0; bottom: 0; right: 0; width: 200%; height: 200%; border: 1px solid #999; -webkit-transform: scale(.5); -webkit-transform-origin: 0 0; -webkit-box-sizing: border-box; }
```
[scale](https://github.com/gaostore/codes/blob/master/scale/index.html)

3.linear-gradient方式：通过渐变实现

html代码：
```html
<div class="wrap">
    <h2>linear-gradient</h2>
    <div class="box"></div>
</div>
```
css代码:
```css
.box { margin: 0 auto; height: 200px; width: 200px; background: #fff; background-image: -webkit-linear-gradient(top, transparent 50%, #999 50%); -webkit-background-size: 100% 1px; background-repeat: no-repeat; background-position: bottom center; border-top: 0 none; padding-bottom: 1px; }
```
[linear-gradient](https://github.com/gaostore/codes/blob/master/linear-gradient/index.html)
