### 640写法 适配
```html
<meta name="viewport" content="width=device-width; initial-scale=1; minimum-scale=1; maximum-scale=1; user-scalable=no;"/>
```

```js
!function () {
    var doc = document.documentElement,
            winW = window.innerWidth,
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

### 适用于微信的适配
```js
function setMeta() {
    var dpr = navigator.appVersion.match(/Mobile/gi) ? window.devicePixelRatio : 1;
    var scale = 1 / dpr;
    var docEl = document.documentElement;
    var metaEl = document.querySelector('meta[name="viewport"]');
    if (!metaEl) {
        metaEl = document.createElement('meta');
        docEl.firstElementChild.appendChild(metaEl);
    }
    metaEl.setAttribute('content', 'width=device-width, initial-scale=' + scale + ', minimum-scale=' + scale + ', maximum-scale=' + scale + ', user-scalable=no');
}
```


### 预加载图片
```js
/**
 * 预加载图片
 * @param imgPath {string} 图片路径
 * @param imgList {array} 加载图片数组
 * @param oLoading {object} 进度提示框
 * @param oLoadTXT {object} 进度提示文字
 */
preLoadImg: function (imgPath, imgList, oLoading, oLoadTXT) {
    var i = 0, len = imgList.length;
    for (; i < len; i++) {
        imgList[i] = imgPath + imgList[i];
    }
    var imgLoader = function (imgs, callback) {
        var len = imgList.length, i = 0;
        while (imgList.length) {
            loadImage(imgList.shift(), function (path) {
                callback(path, ++i, len);
            });
        }
    };

    imgLoader(imgList, function (path, curNum, total) {
        var percent = Math.floor((curNum / total).toFixed(2) * 100);
        oLoadTXT.innerHTML = percent + '%';
        percent == 100 && oLoading && document.body.removeChild(oLoading);
    });

    function loadImage(path, callback) {
        var img = new Image();
        img.onload = function () {
            img.onload = null;
            callback(path);
        };
        img.src = path;
    }
}
```

### 屏幕旋转事件
```js
// @param fn {function} callback
function orientationHandler(fn) {
    var win = window;
    var supportsOrientation = (typeof win.orientation == 'number' && typeof win.onorientationchange == 'object');
    var orientationEvent = supportsOrientation ? 'orientationchange' : 'resize';
    var HTMLNode = document.body.parentNode;
    var updateOrientation = function () {
        if (supportsOrientation) {
            updateOrientation = function () {
                var orientation = win.orientation;
                switch (orientation) {
                    case 90:
                    case -90:
                        orientation = 'landscape';
                        break;
                    default:
                        orientation = 'portrait';
                }
                HTMLNode.setAttribute('class', orientation);
            }
        } else {
            updateOrientation = function () {
                var orientation = (win.innerWidth > win.innerHeight) ? 'landscape' : 'portrait';
                HTMLNode.setAttribute('class', orientation);
            }
        }
        typeof fn === 'function' && fn();
        updateOrientation();
    };
    win.addEventListener(orientationEvent, updateOrientation, false);
}
```

### 判断是否是微信
```js
function isWX() {
    return navigator.userAgent.toLowerCase().match(/MicroMessenger/i) == "micromessenger";
}
```

### guideTop
```html
<a href="javascript:;" data-index="2" class="u-guideTop z-move"><i></i></a>
```
```css
.u-guideTop { position: absolute; bottom: 0; left: 50%; z-index: 1; width: 200px; height: 70px; border-radius: 8px 8px 0 0; background-color: rgba(0, 0, 0, .2); color: #fff; -webkit-transform: translate3d(-50%, 0, 0); }
.u-guideTop i { position: absolute; top: 50%; left: 50%; margin: -26px 0 0 -35px; width: 70px; height: 26px; background: url("../images/guideTop.png") no-repeat 50% 50%; }
.u-guideTop.z-move i { -webkit-animation: guideTop 1.5s infinite }
@-webkit-keyframes guideTop {
    0% { -webkit-transform: translateY(64px); opacity: 0 }
    60% { -webkit-transform: translateY(18px); opacity: 1 }
    100% { -webkit-transform: translateY(0px); opacity: 0 }
    }
@keyframes guideTop {
    0% { transform: translateY(64px); opacity: 0 }
    60% { transform: translateY(18px); opacity: 1 }
    100% { transform: translateY(0px); opacity: 0 }
    }
```
