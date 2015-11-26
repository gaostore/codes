### 640写法 适配
```html
这一行不用写，脚本自动生成
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

### 适用于微信的适配 方法1
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
### 适用于微信的适配 方法2
```js
(function () {
    var version, phoneScale;
    /Android (\d+\.\d+)/.test(navigator.userAgent) ? (version = parseFloat(RegExp.$1), version > 2.3 ? (phoneScale = parseInt(window.screen.width) / 640, document.write('<meta name="viewport" content="width=640, minimum-scale = ' + phoneScale + ", maximum-scale = " + phoneScale + ', target-densitydpi=device-dpi">')) : document.write('<meta name="viewport" content="width=640, target-densitydpi=device-dpi">')) : document.write('<meta name="viewport" content="width=640, user-scalable=no, target-densitydpi=device-dpi">');
})();
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
//or 一开始就记载音乐
<div class="music-bar" id="j_musicBar">
    <div class="music-img"></div>
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
or
```css
.music-bar { position: absolute; top: 60px; right: 30px; z-index: 4; width: 60px; height: 60px; -webkit-transform-style: preserve-3d; -webkit-backface-visibility: hidden; }
.music-img { position: absolute; top: 0; left: 0; width: 60px; height: 60px; background: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADwAAAA8CAMAAAANIilAAAAAb1BMVEUAAAAAAABSUlL///////8uLi7////X19eFhYX///+qqqr////19fW4uLj////Ozs7u7u7ExMT////////g4ODu7u7o6Ojg4OD////5+fnn5+f////Ozs7///+3t7dubm7///////+ZmZn///////+fNR61AAAAJHRSTlNNAGMPv1g/vXqvkM/pm++y3qaPb8jd08d/9dLfsaCab1BPhYCvMKJ8AAACRUlEQVRIx6TT23KiQBhF4S19oCGIo4YxEmOsyXr/ZxwsnElM/4BVWZdUfbU5NFrZxY83/wrw6t8+4srOwm7v+Zbfu4dwrDCr4iKOt9HudE6Fhop0PnW3+TiL3bja10F3hbof1900vrQAZSGjogRoL1N4l1GD70zsnoAmaabUAE8uw6M9BM0WDndaX+1Zi51HfY+vtlYW7/pWfdX3eGdbkWTp3Vd8AdYyYqusNXD5xK6Fg6x4Vt4BWvcfV9AEGx+N66GB6h+OQJIZbJWXgHjDHkrZwfFdeSX4EUegmMT8CsoqxukBV9PDYkKXUF2xA4o5zLM97Qa8h15T0YN5Yz3sB+yhnsZS3cDGOmd+wECYwwon+JN9a2ClCJ0m4zZzzN5KB1EvsFnC2uaPvYEXVbBexOoo8t+jkoe0jBPb/Ih6tVAsYzW/8y/dCtADuMQ4P4/i9U9w+hH+W3vZrDAMAkFYAgmlKCqxNoVeDPv+z1gKljnsyAqhXsOHQdf5eRB49sBCIQc2eVUuBnJVk0PiEhuSufF0Cx/PTSTZcFavNols5pPsM7KSJ6nEgMP3PBCDXaRa8Kk+VJF9KIBL9CIpd5gL4Eh6o/SlOUgvF/01iYKJ6HO7qWLAsBtldBnsS3EwOm6xHjD+iFgszL3gI1ju2gXm/l0NsQI7w151rGjDQHN2FqmMBxoepW5PEf/GtipKsRAXnLkCCXGdLlZ8LGCvBdfrkRmr2WG9mTWh6ppQURPsgpJi+BWUEJNZUOxq9NdShv0P1MFjVAc/zQlegd4Ks4gAAAAASUVORK5CYII=") no-repeat 50% 50%; -webkit-background-size: contain; background-size: contain; }
.play .music-img { -webkit-animation: rotate 1.2s linear infinite; animation: rotate 1.2s linear infinite; }
.pause .music-img { background: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADwAAAA8CAMAAAANIilAAAAAZlBMVEUAAAAAAABSUlL///8uLi7X19eFhYX///////+4uLj19fXu7u7///+qqqr////////o6Oj////Ozs7ExMT////////////////g4ODg4OD5+fn///////////9ubm7Ozs6ZmZn///+mokGXAAAAIXRSTlNNAGMPWL16v6+b6d7PkD/v00Cypo9/b0/Ix/XfwKBvsYXSEmXdAAACbUlEQVRIx6XX25KiMBAG4BYi4SAoIIqMh8n7v+S2FfGfTHeyW2tfTE2RfNVWOkfa6NFPt+uX4/i63qY+0knD+enqfsX1lP8T7kunRtnHMKj1XZtHVWTEkRXVo/HfbJ/Euc96aA0FYdqDz57H8XnvOLYZKZFtHcf+HMNH0Dg/qjjfcVNdUCKKmrvscuDAXgwlw1wcRw78w1b013DIDfy0rdL3Li3rEB+9lbiQ1o8a8Dn2m90oLFX85wyc73msSMWzsEQXrnf+xiXXyOh4McKS4YqVK+75YxEb3FFYooL/61/Y8ryiGF7uwhLxXLNP7BNnUewaIyxlPjXjEokl9hoWqcsnzhOJPZphf6bOGZ94/VIUH9waYQN/PzG2mFsymLQ1LKLlIWPMDSaFSbNk+NuGOh4RSmBvFzEqjXMdTc4NCeztKOsxODdRmVzG61g1oiAVF4usnJrKWijC5eWnqKU9qhy1HPW7nKj0/tmetuuUUhqBk5aqJE5bKpJYt8CNhmMD5ukbV7MyYCiVZoGHSikVJolmgRt1kmB6Sguc6dMTC0Na4LFVFwaWpLTAW6MtSWwG0gK3I2mbAbYhaYG/A4htCBtgYLOh5sEYHSHEBoitN7SDuu3JrXfTvVLDmsYBxxJ3OG6CvAeXxjhufOqCYEfYRbWFTxwcsWuqGngrJY5YHO4zagSrn9rzerjjWvG2yLzcFYprBS40sPT9svqtrOWWo7xKYdu4OFc/7hSzu/+8xFXyErfq2aSpmWE/uLh+dmX+/LL++TMB0a0PlAEPlGF9oHSxBwp47GkEmn6U2d/Sph5lMv90s/45aG8TcobxB3U0b0cPT2KZAAAAAElFTkSuQmCC") no-repeat 50% 50%;-webkit-background-size: contain; background-size: contain;  }
@-webkit-keyframes rotate {
    0% { -webkit-transform: rotate(0deg) }
    99% { -webkit-transform: rotate(-360deg) }
}
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

//or
/**
 * Music Mood
 * @type {Element}
 */
var oMusic = document.getElementById('j_musicBar');
var audio = document.querySelector("audio");
play();
var isPlay = true;
function pause() {
    audio.pause();
    isPlay = false;
    oMusic.classList.remove('play');
    oMusic.classList.add('pause');
}
function play() {
    audio.play();
    isPlay = true;
    oMusic.classList.add('play');
    oMusic.classList.remove('pause');
}
oMusic.addEventListener('touchstart',function(){
    isPlay? pause():play();
});
```

###判断访问终端
```js
var browser={
    versions:function(){
        var u = navigator.userAgent, app = navigator.appVersion;
        return {
            trident: u.indexOf('Trident') > -1, //IE内核
            presto: u.indexOf('Presto') > -1, //opera内核
            webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
            gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1,//火狐内核
            mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
            ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
            android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或者uc浏览器
            iPhone: u.indexOf('iPhone') > -1 , //是否为iPhone或者QQHD浏览器
            iPad: u.indexOf('iPad') > -1, //是否iPad
            webApp: u.indexOf('Safari') == -1, //是否web应该程序，没有头部与底部
            weixin: u.indexOf('MicroMessenger') > -1, //是否微信 （2015-01-22新增）
            qq: u.match(/\sQQ/i) == " qq" //是否QQ
        };
    }(),
    language:(navigator.browserLanguage || navigator.language).toLowerCase()
}
```


###判断是否安装过app
```html
<meta name='apple-itunes-app' content='app-id=你的APP-ID'>
<a href="javascript:;" id="openApp" onclick="testApp()"></a>
```
```js
 var log = function (msg) {
        $('body').before('<div class="log">' + msg + '</div>');
    };
    var timeout, t = 1000, hasApp = true;
    setTimeout(function () {
        if (hasApp) {
            log('安装了app');
            $('#dl_app').hide();

        } else {
            log('未安装app');
            $('#dl_app').show();
            forceDownload();
        }
    }, 2000)
    function testApp() {
        var t1 = Date.now();
        var ifr = $('<iframe id="ifr"></iframe>')
        ifr.attr('src',您们app的协议);
        $('body').append(ifr);
        timeout = setTimeout(function () {
            try_to_open_app(t1);
        }, t);
    }
    function try_to_open_app(t1) {
        var t2 = Date.now();
        if (!t1 || t2 - t1 < t + 200) {
            hasApp = false;
        }
    }
    testApp();
```
