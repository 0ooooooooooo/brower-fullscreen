<template></template>
<script>
export default {
    name: 'javascript-fullscreen',
    data () {
        return {
            isFull: false
        }
    },
    created () {
        // 进入取消监听事件
        this.handleEvenetListener(false);
        // 目前操作为拦截F11事件，执行H5事件
        // 从大屏页面全屏状态切换到该页面时需要判断
        this.isFull = this.judgeBrowserFullScreen();
        // F11拦截
        this.handleEvenetListener(true);
    },
    beforeDestroy () {
        this.handleEvenetListener(false);
    },
    methods: {
        /**
         * @name
         * @desc 设置键盘按键监听事件
         * @param { Boolean } status true->addEventListener/false->removeEventListener
         * @return { undefined } 无返回值
        */
        handleEvenetListener (status) {
            let _this = this;
            const keydownFn = function (e) {
                // 全屏后的window对象更改，不会触发当前的keydown事件
                // if (e.code === 'F11' && _this.isFull) {
                //    e.preventDefault();
                //    _this.operaExitFullScreen();
                // } else 
                // 当前未全屏页面的打开事件
                if (e.code === 'F11' && !_this.isFull) {
                    e.preventDefault();
                    _this.operaFullScreen();
                };
            }
            // 监听浏览器窗口变化事件，更改是否全屏的值
            const resize = function(e) {
                _this.isFull = _this.judgeBrowserFullScreen();
            };
            if (status) {
                window.addEventListener('keydown', keydown);
                window.addEventListener('resize', resize);
            } else {
                window.removeEventListener('keydown', keydown);
                window.removeEventListener('resize', resize);
            }
        },
        // 判断是否浏览器全屏
        judgeBrowserFullScreen () {
            const explorer = window.navigator.userAgent.toLowerCase();
            if(explorer.indexOf('chrome')>0){ //webkit
                if (document.body.scrollHeight === window.screen.height && document.body.scrollWidth === window.screen.width) {
                    return true;
                } else {
                    return false;
                }
            }else{ //IE 9+  fireFox
                if (window.outerHeight === window.screen.height && window.outerWidth === window.screen.width) {
                    return true;
                } else {
                    return false;
                }
            }
        },
        // 手动打开全屏
        operaFullScreen() {
            let el = document.documentElement;
            let rfs =
                el.requestFullScreen ||
                el.webkitRequestFullScreen ||
                el.mozRequestFullScreen ||
                el.msRequestFullscreen
            if (rfs) {
                rfs.call(el);
            } else if (typeof window.ActiveXObject !== 'undefined') {
                // for IE，这里其实就是模拟了按下键盘的F11，使浏览器全屏
                let wscript = new ActiveXObject('WScript.Shell');
                if (wscript != null) {
                    wscript.SendKeys('{F11}');
                }
            }
        },
        // 手动关闭全屏
        operaExitFullScreen() {
            let el = document;
            let cfs =
                el.cancelFullScreen ||
                el.mozCancelFullScreen ||
                el.msExitFullscreen ||
                el.webkitExitFullscreen ||
                el.exitFullscreen;
            if (cfs) {
                cfs.call(el);
            } else if (typeof window.ActiveXObject !== 'undefined') {
                // for IE，这里和fullScreen相同，模拟按下F11键退出全屏
                let wscript = new ActiveXObject('WScript.Shell');
                if (wscript != null) {
                    wscript.SendKeys('{F11}');
                }
            }
        }
    }
}
</script>
