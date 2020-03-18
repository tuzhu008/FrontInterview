# Symbol

symbol 是一种基本数据类型 （primitive data type）。`Symbol()` 函数会返回 symbol 类型的值，该类型具有静态属性和静态方法。

每个从 `Symbol()` 返回的 symbol 值都是**唯一**的。一个 symbol 值能作为对象属性的标识符；这是该数据类型仅有的目的。

它的静态属性会暴露几个内建的成员对象；它的静态方法会暴露全局的symbol注册，且类似于内建对象类，但作为构造函数来说它并不完整，因为它不支持语法："new Symbol()"。

## 简单介绍一下 symbol

## `Symbol.for(key)`

`Symbol.for(key)` 方法会根据给定的键 key，来从运行时的 **symbol 注册表**中找到对应的 symbol，如果找到了，则返回它，否则，新建一个与该键关联的 symbol，并放入全局 symbol 注册表中。

和 `Symbol()` 不同的是，用 `Symbol.for()` 方法创建的的 symbol 会被放入一个全局 symbol 注册表中。

`Symbol.for(key)` 创建的 symbol 可以跨文件，跨域。

## `Symbol.keyFor(sym)`

`Symbol.keyFor(sym)` 方法用来获取 symbol 注册表中与某个 symbol 关联的键。