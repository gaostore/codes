# 三种实现Retina屏的1px
1.svg方法
```css
.box { margin: 0 auto; height: 200px; width: 200px; background: #fff;}
        .box1 { border-left: 0; background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1' height='100%'><rect fill='#c8c8c8' x='0' y='0' width='0.5' height='100%'/></svg>"); background-position: 0 0; background-repeat: no-repeat; }
        .box2 { border-top: 0; background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100%' height='1'><rect fill='#c8c8c8' x='0' y='0' width='100%' height='0.5'/></svg>"); background-position: 0 0; background-repeat: no-repeat; }
        .box3 { border-right: 0; background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1' height='100%'><rect fill='#c8c8c8' x='0' y='0' width='0.5' height='100%'/></svg>"); background-position: 100% 0; background-repeat: no-repeat; }
        .box4 { border-bottom: 0; background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100%' height='1'><rect fill='#c8c8c8' x='0' y='0' width='100%' height='0.5'/></svg>"); background-position: 0 100%; background-repeat: no-repeat; }
        .box5 { background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1' height='100%'><rect fill='#c8c8c8' x='0' y='0' width='0.5' height='100%'/></svg>"),url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1' height='100%'><rect fill='#c8c8c8' x='0' y='0' width='0.5' height='100%'/></svg>"); background-position: 0 0, 100% 0; background-repeat: no-repeat; }
        .box6 { background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100%' height='1'><rect fill='#c8c8c8' x='0' y='0' width='100%' height='0.5'/></svg>"),url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100%' height='1'><rect fill='#c8c8c8' x='0' y='0' width='100%' height='0.5'/></svg>"); background-position: 0 0,0 100%; background-repeat: no-repeat; }
        .box7 { background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg'><rect x='0' y='0' rx='3' ry='3' width='100%' height='100%' style='fill:#fff; stroke:#dcdcdc; stroke-width: dd0.5;fill-opacity:0;'/></svg>"); background-position: 0 0; background-repeat: no-repeat; }
```
[test](http://www.baidu.com)
