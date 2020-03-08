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

## 实现一个 once 函数，传入函数参数只执行醒一次？

```js
function once (func) {
    var tag = true;

    return function () {
        if (tag === true) {
            tag = false;
            return func.apply(null, arguments);
        }
    }
}
```

## 将原生的 ajax 封装成 promise

```js
function ajax (options) {
    const { method = 'GET', url, headers = {}, timeout = 20000, data } = options;
    return new Promise(function (resolve, reject) {
        const xhr = new XMLHttpRequest();
        const headerEntries = Object.entries(headers);
        let requestURL = url;
        let requestBody = data;

        if (method === 'GET' || method === 'HEAD') {
            requestURL = encode(url, data);
            requestBody = null;
        } else {
            requestBody = JSON.parse(data);
        }

        xhr.open(method, url);

        for (let [key, value] of headerEntries) {
            xhr.setRequestHeader(key, value);
        }

        xhr.onreadystatechange = function () {
            if (xhr.readyState === XMLHttpRequest.DONE) {
                if (xhr.status !== 200) {
                    resolve(JSON.parse(xhr.responseText));
                } else {
                    reject();
                }

            }
        }

        xhr.timeout = function () {
            reject('超时');
        }

        xhr.send(requestBody);
    });
}
```

## JS 监听对象属性的改变

* 在 ES5 中可以用过 `Object.defineProperty` 来实现已有属性的监听

  ```js
    Object.defineProperty(user, 'name', {
        set: function (key, value) {}
    });
  ```

  缺点：如果 id 不再 user 对象中，则不能监听 id 的变化

* 在 ES6 中可以通过 Proxy 来实现

  ```js
    var user = new Proxy({}, {
        set: function (target, key, value, receiver) {

        }
    })
  ```

  这样即使有属性 user 中不存在，通过 user.id 来定义同样也可以这样监听到这个属性的变化哦

## 如何实现一个私有变量，用 getName 方法可以访问，不能直接访问

* 闭包

  ```js
    function User (name) {
        const name = name;

        this.getName = function () {
            return name;
        }
    }
  ```

## 实现 sleep 的效果？

* while
   
    ```js
    function sleep (time) {
        const start = Date.now();
        const expired = start + time;

        while (Date.now() > expired) {
            return;
        }
    }
    ```

* promise

    ```js
    function sleep (time) {
        return new Promise((reslove) => setTimeout(reslove, time));
    }
    ```

* async

    ```js
    async function sleep (time) {
        const result = await new Promise((reslove) => setTimeout(reslove, time));
        return result;
    }
    ```

* generate

    ```js
    function* sleep (time) {
        yield new Promise((reslove) => setTimeout(reslove, time));
    }

    sleep(500).next().value.then(() => { /* do something */ })
    ```


## JS 操作获取和设置 cookie

## 防抖和节流

防抖：抖动停止后操作设定的时间时执行一次函数。

节流：按照设定的时间间隔执行函数

