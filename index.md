### Hello!

Welcom to my GitHub Pages. 

It is a site for recording my usual exercises.

Make progress every day!


#### 1.Progress Indicator

`基于原生JavaScipt的顶部进度条插件，兼容到IE6`

```javascript
(function () {
  var root =
    (typeof self == "object" && self.self === self && self) ||
    (typeof global == "object" && global.global === global && global) ||
    this ||
    {};

  var lastTime = 0;
  var vendors = ["webkit", "moz"];
  for (var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
    window.requestAnimationFrame = window[vendors[x] + "RequestAnimationFrame"];
    window.cancelAnimationFrame = window[vendors[x] + "CancelAnimationFrame"] || window[vendors[x] + "CancelRequestAnimationFrame"];
  }

  if (!window.requestAnimationFrame) {
    window.requestAnimationFrame = function (callback, element) {
      var currTime = new Date().getTime();
      var timeToCall = Math.max(0, 16.7 - (currTime - lastTime));
      var id = window.setTimeout(function () {
        callback(currTime + timeToCall);
      }, timeToCall);
      lastTime = currTime + timeToCall;
      return id;
    };
  }
  if (!window.cancelAnimationFrame) {
    window.cancelAnimationFrame = function (id) {
      clearTimeout(id);
    };
  }

  var uitls = {
    extend: function (target) {
      for (var i = 1, len = arguments.length; i < len; i++) {
        for (var prop in arguments[i]) {
          if (arguments[i].hasOwnProperty(prop)) {
            target[prop] = arguments[i][prop];
          }
        }
      }
      return target;
    },
    getViewHeight: function () {
      return window.innerHeight || document.documentElement.clientWidth || document.body.clientWidth || 0;
    },
    getScrollTop: function () {
      return window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0;
    }
  };

  function ProgressIndicator(options) {
    this.defaultOptions = { color: "#0A7ADA", height: 3 };
    this.options = uitls.extend({}, this.defaultOptions, options);
    this.init();
  }
  ProgressIndicator.prototype = {
    constructor: ProgressIndicator,
    init: function () {
      this.createIndicator();
      var width = this.calculateWidthPrecent();
      this.setWidth(width);
      this.bindScrollEvent();
    },
    createIndicator: function () {
      var _div = document.createElement("div");
      _div.className = "progress-indicator";
      _div.style.position = "fixed";
      _div.style.top = 0;
      _div.style.left = 0;
      _div.style.height = parseInt(this.options.height) + "px";
      _div.style.backgroundColor = this.options.color;
      this.element = _div;
      document.body.appendChild(_div);
    },
    setWidth: function (percent) {
      this.element.style.width = percent * 100 + "%";
    },
    calculateWidthPrecent: function () {
      this.docHight = Math.max(document.documentElement.scrollHeight, document.documentElement.clientHeight);
      this.viewHeight = uitls.getViewHeight();
      this.sHeight = Math.max(this.docHight - this.viewHeight, 0);
      var scrollTop = uitls.getScrollTop();
      return this.sHeight ? scrollTop / this.sHeight : 0;
    },
    bindScrollEvent: function () {
      var self = this;
      var prev;
      window.onscroll = function () {
        window.requestAnimationFrame(function () {
          var percent = Math.min(uitls.getScrollTop() / self.sHeight);
          if (percent == prev) return;
          prev = percent;
          self.setWidth(percent);
        });
      };
    }
  };
  if (typeof exports != "undefined" && !exports.nodeType) {
    if (typeof module != "undefined" && !module.nodeType && module.exports) {
      exports = module.exports = ProgressIndicator;
    }
    exports.ProgressIndicator = ProgressIndicator;
  } else {
    root.ProgressIndicator = ProgressIndicator;
  }
})();

```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      html,
      body {
        margin: 0;
        padding: 0;
      }
      body {
        height: 3000px;
      }
    </style>
    <script src="./progress-indicator.js"></script>
  </head>
  <body>
    <script>
      var progressBar = new ProgressIndicator({});
    </script>
  </body>
</html>
```
#### 2.td-message

[基于jQuery的消息弹窗插件](https://github.com/shitudouya/td-message)

