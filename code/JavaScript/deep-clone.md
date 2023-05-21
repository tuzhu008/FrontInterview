## 要点
* 深度
* 可枚举属性和自身属性
* 循环引用
  * weakmap
* 性能优化
* 垃圾回收
* 类型

## 数据类型

### 可深度遍历类型

* Map
* Set
* Array
* Object
* Arguments

### 不可深度遍历类型

* Boolean
* Date
* Number
* String
* Symbol
* Error
* RegExp
* Function
* undefined
* null
### 可直接返回类型

* Boolean
* Number
* String
* undefined
* null
### 不可直接返回类型

* Map
* Set
* Array
* Object
* Arguments
* Date
* Symbol
* Error
* RegExp
* Function

## JSON 实现
```javascript
JSON.parse(JSON.stringify());
```

## 算法分解
### 克隆对象
### 克隆数组
### 克隆函数
### 克隆 `Symbol`
```javascript
function cloneSymbol(targe) {
    return Object(Symbol.prototype.valueOf.call(targe));
}
```
经过此方法的克隆的 Symbol  `Object.prototype.toString.call`值为 `[object Symbol]`，但其 `typeof`值变为了 `object`
### 克隆正则
```javascript
function cloneReg(regexp) {
  const newReg = new RegExp(regexp.source, regexp.flags);
  newReg.lastIndex = regexp.lastIndex;
  return newReg;
}
```
## 算法
```javascript
function deepClone(obj) {
  const map = new Map();

  const clone = (target) => {
    if (map.has(target)) {
      return map.get(target);
    }
    const type = Object.prototype.toString.call(target).replace(/^\[object\s(.*)]$/ig, '$1');

    const strategy = {
      Array() {
        const arr = [];
        for(let i = 0, length = target.length; i < length; i++) {
          arr[i] = clone(target[i]);
        }

        return arr;
      },
      Object() {
        const obj = {};

        for(let key of target) {
          obj[key] = clone(target[key]);
        }
      },
      Map() {
        const newMap = new Map();
        target.forEach((v, k) => {
          newMap.set(clone(k), clone(v));
        });

        return newMap;
      },
      Set() {
        const newSet = newSet();
        target.forEach((v) => {
          newSet.set(clone(v));
        });

        return newSet;
      },
      Date() {
        return new Date(target.valueof());
      },
      RegExp() {
        const newReg = new RegExp(target.source, target.flags);
        newReg.lastIndex = target.lastIndex;
        return newReg;
      },
      Error() {
        return new Error(target.message);
      }
    };

    if (strategy[type]) {
      const result = strategy[type](target);
      map.set(target, result);
      return result;
    }

    return target;
  }
}
```
