## 基础数据类型
* Undefined
* Null
* Number
* String
* Boolean
* Symbol
* BigInt

## 引用数据类型

* Object
* Function
* Array
* Date
* RegExp
* Error

## 判断类型

### typeof

* "undefined"
* "string"
* "number"
* "boolean"
* "symbol"
* "function"
* "bigint"
* "object"

| 类型 | 结果 |
| --- | --- |
| Undefined | "undefined" |
| Null | "object" |
| Boolean | "boolean" |
| Number | "number" |
| BigInt | "bigint" |
| String | "string" |
| Symbol | "symbol" |
| Function | "function" |
| 其他任何对象 | "object" |

### intanceof
判断对象
### Object.prototype.toString.call
准确判断任何值

| 类型 | 结果 |
| --- | --- |
| Object.prototype.toString.call(undefined) | '[object Undefined]' |
| Object.prototype.toString.call(null) | '[object Null]' |
| Object.prototype.toString.call(true) | "[object Boolean]" |
| Object.prototype.toString.call(123) | '[object Number]' |
| Object.prototype.toString.call('') | "[object String]" |
| Object.prototype.toString.call([]) | "[object Array]" |
| Object.prototype.toString.call({}) | "[object Object]" |
| Object.prototype.toString.call(BigInt(123)) | '[object BigInt]' |
| Object.prototype.toString.call(Symbol()) | '[object Symbol]' |
| Object.prototype.toString.call(new Date()) | "[object Date]" |
| Object.prototype.toString.call(/123/) | "[object RegExp]" |
| Object.prototype.toString.call(new Error()) | "[object Error]" |
| Object.prototype.toString.call(() => {}) | "[object Function]" |
| Object.prototype.toString.call(Math) | "[object Math]" |
| Object.prototype.toString.call(JSON) | "[object JSON]" |

## 数据存储

- **值类型**是直接存储在**栈（stack）**中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
- **引用类型**存储在**堆（heap）**中的对象，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；
