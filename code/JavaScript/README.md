# 代码

## 写一个通用的事件监听函数？

```js
MyEvent = {
    // 页面加载完成后
    readyEvent: function(fn) {
        if (fn == null) {
            fn = document;
        }

        if (typeof window.onload !== 'function') {
            window.onload = fn;
        } else {
          var oldonload = window.onload;
            window.onload = function() {
                oldonload();
                fn();
            };
        }
    },
    // 视能力分别使用dom0||dom2||IE方式 来绑定事件
    // 参数： 操作的元素,事件名称 ,事件处理程序
    addEvent: function(element, type, handler) {
        if (element.addEventListener) {
            //事件类型、需要执行的函数、是否捕捉
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent) {
            element.attachEvent('on' + type, function() {
                handler.call(element);
            });
        } else {
            element['on' + type] = handler;
        }
    },
    // 移除事件
    removeEvent: function(element, type, handler) {
        if (element.removeEnentListener) {
            element.removeEnentListener(type, handler, false);
        } else if (element.datachEvent) {
            element.detachEvent('on' + type, handler);
        } else {
            element['on' + type] = null;
        }
    },
    // 阻止事件 (主要是事件冒泡，因为IE不支持事件捕获)
    stopPropagation: function(ev) {
        if (ev.stopPropagation) {
            ev.stopPropagation();
        } else {
            ev.cancelBubble = true;
        }
    },
    // 取消事件的默认行为
    preventDefault: function(event) {
        if (event.preventDefault) {
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    },
    // 获取事件目标
    getTarget: function(event) {
        return event.target || event.srcElement;
    },
    // 获取event对象的引用，取到事件的所有信息，确保随时能使用event；
    getEvent: function(e) {
        var ev = e || window.event;
        if (!ev) {
            var c = this.getEvent.caller;
            while (c) {
                ev = c.arguments[0];
                if (ev && Event === ev.constructor) {
                    break;
                }
                c = c.caller;
            }
        }
        return ev;
    }
};
```

## 深度克隆

```js
function clone (origin) {
    var buf;
    if (origin instanceof Array) {
        buf = [];
        for (let i = 0; i < origin.length; i++) {
            buf[i] = clone(origin[i]);
        }
        return buf;
    } else if (origin instanceof Object) {
        buf = {};
        for (let k in origin) {
            buf[k] = clone(origin[k]);
        }

        retrun buf;
    } else {
        return origin;
    }
}
```

## 数组去重

重点在于去重算法：

* Set

  ```js
    function uniqArray = function (arr) {
        retrun Array.from(new Set(arr));
    }
  ```

* filter

  ```js
    function uniqArray = function (arr) {
        retrun arr.filter((item, index) => arr.indexOf(item) === index);
    }
  ```

```js
if (!Array.prototype.uniqArray) {
    Array.prototype.uniqArray = function () {
        const newArr = uniqArray(this);
        this.length = 0;
        for (let i = 0; i < newArr.length; i++) {
            this[i] = newArr[i];
        }
    }
}
```

## JS 操作获取和设置 cookie

## 防抖和节流

防抖：抖动停止后操作设定的时间时执行一次函数。

节流：按照设定的时间间隔执行函数