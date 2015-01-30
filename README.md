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
