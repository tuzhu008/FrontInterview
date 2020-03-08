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

## 垃圾回收

JavaScript 中的内存管理是自动执行的，而且是不可见的。我们创建基本类型、对象、函数……所有这些都需要内存。

### 什么是垃圾

一般来说没有被引用的对象就是垃圾，就是要被清除，例如有个例外如果几个对象引用形成一个环，互相引用，但根访问不到它们，这几个对象也是垃圾，也要被清除。

### 回收机制

JS 的垃圾回收机制有两种：标记清除、引用计数

* **标记清除：**

  * 标记阶段：从根集合出发，将所有活动对象及其子对象打上标记
  * 清除阶段：遍历堆，将非活动对象（未打上标记）的连接到空闲链表上

* **引用计数：**

    引用计数，就是记录每个对象被引用的次数，每次新建对象、赋值引用和删除引用的同时更新计数器，如果计数器值为 0 则直接回收内存。

    *优点：*
    * 可即刻回收垃圾
    * 最大暂停时间短
    * 没有必要沿指针查找， 不要和标记-清除算法一样沿着根集合开始查找
  
    *缺点：*
    * 计数器的增减处理繁重
    * 计数器需要占用很多位
    * 实现繁琐复杂， 每个赋值操作都得替换成引用更新操作
    * 循环引用无法回收


[几种垃圾回收算法](https://www.jianshu.com/p/a8a04fd00c3c)
[V8 之旅： 垃圾回收器](http://newhtml.net/v8-garbage-collection/)
