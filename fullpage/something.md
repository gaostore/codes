###640写法 适配头部
```html
<meta name="viewport" content="width=device-width; initial-scale=1; minimum-scale=1; maximum-scale=1; user-scalable=no;"/>
```
```js
    !function () {
        var doc = document.documentElement,
                winW = doc.clientWidth,
                scale = winW / 640,
                head = doc.firstElementChild,
                meta = head.querySelector('meta[name="viewport"]');
        if (meta) {
            meta = document.createElement("meta");
            meta.setAttribute("name", "viewport");
            doc.firstElementChild.appendChild(meta);
        }
        meta.setAttribute("content", "width=device-width; initial-scale=" + scale + "; maximum-scale=" + scale + "; minimum-scale=" + scale + "; user-scalable=no");
    }();
```
