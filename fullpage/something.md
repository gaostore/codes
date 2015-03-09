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
```html
<div class="page-loading-wrap" id="j_pageLoading">
    <div class="time-line-txt">正在加载中<em id="j_progress"></em></div>
</div>
```
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

###音乐
```html
<div class="music-bar" id="j_musicBar">
    <div class="music-img"></div>
    <div class="music-txt">开启</div>
    <audio id="mp3" loop preload="auto" autoplay="true" class="hide" src=""></audio>
</div>
```
```css
.music-bar { position: absolute; top: 20px; right: 20px; z-index: 4; width: 60px; height: 60px }
.music-img { position: absolute; top: 0; left: 0; width: 48px; height: 48px; background: url("../images/music.png?__sprite") no-repeat 50% 50%; }
.play .music-img { -webkit-animation: rotate 1.2s linear infinite; animation: rotate 1.2s linear infinite }
.music-txt { position: absolute; top: 0; left: 0; color: #fff; font-size: 28px; line-height: 50px; opacity: 0; -webkit-animation-duration: 1.2s; -webkit-animation-timing-function: ease }
.play .music-txt { -webkit-animation-name: fadeinr }
.pause .music-txt { -webkit-animation-name: fadeinr; -webkit-animation-delay: .01s }
```
```javascript
// 音乐
window.NS = window.NS || {};
NS._util = {
    hasClass: function (obj, className) {
        return RegExp('(^|\\s)' + className + '(\\s|$)').test(obj.className);
    }
    , addClass: function (obj, newClass) {
        if (!NS._util.hasClass(obj, newClass)) {
            obj.className += (obj.className ? ' ' : '') + newClass;
        }
    }
    , removeClass: function (obj, curClass) {
        obj.className = obj.className.replace(RegExp('(\\s|$)' + curClass, 'g'), '');
    },
    /**
     * 查找键值
     * @param name {string} 键
     * @returns {*|null}
     */
    getData: function (name) {
        var str = localStorage.getItem(name);
        var data = JSON.parse(str);
        return data || null;
    },
    /**
     * 保存数据
     * @param name {string} 键
     * @param property {string} 属性
     * @param value {string} 值
     */
    setData: function (name, property, value) {
        var storageData = this.getData(name) || {};
        storageData[property] = value;
        var str = JSON.stringify(storageData);
        localStorage.setItem(name, str);
    }
    }

var oMusic = document.getElementById('j_musicBar');
var oMusicTxt = oMusic.querySelector('.music-txt');
var audio = document.querySelector("audio");
musicUtil(oMusic, oMusicTxt, audio);
function musicUtil(oMusic, oMusicTxt, audio) {
    var isPlay = false;
    audioAttach();

    oMusic.addEventListener('touchstart', function () {
        isPlay ? pause() : play();
    }, false);

    function audioAttach() {
        var palyFirst = function () {
            audio.play();
            NS._util.addClass(oMusic, 'play');
            isPlay = true;
            document.body.removeEventListener('touchstart', palyFirst, false);
        };

        audio.oncanplay = playmusic();
        audio.autoplay = true;
        audio.isLoadedmetadata = false;
        audio.audio = true;

        function playmusic() {
            document.body.addEventListener('touchstart', palyFirst, false);
        }
    }

    function pause() {
        audio.pause();
        isPlay = false;
        oMusicTxt.innerHTML = '&#26242;&#20572;';
        NS._util.removeClass(oMusic, 'play');
        NS._util.addClass(oMusic, 'pause');
    }

    function play() {
        audio.play();
        isPlay = true;
        oMusicTxt.innerHTML = '&#24320;&#21551;';
        NS._util.removeClass(oMusic, 'pause');
        NS._util.addClass(oMusic, 'play');
    }
}
```


