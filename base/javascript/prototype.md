# 原型

无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个 `prototype` 属性，这个属性指向函数的原型对象。

默认情况下，所有原型对象会自动获得一个 `constructor` 属性，该属性指向这个原型对象所在的函数的指针。

```js
function F() {}

// true
F.constructor === Function

// {constructor: ƒ}
F.prototype
// true
F.prototype.constructor === F
```

创建了自定义构造函数后，其原型对象默认只会获得 `constructor` 属性；至于其他方法，则都是从 Object 继承而来。
从下面的代码可以看出，F 的 `prototype` 属性是 `Object` 的实例

```js
// true
F.prototype.__proto__ === Object.prototype
```

当调用构造函数创建一个实例后，该实例的内部将包含一个指针（内部属性 `__proto__`），指向构造函数的原型对象。

```js
const f = new F();

// true
f.constructor === F

// true
f.__proto__ === F.prototype
```

每当代码读取某个对象的某个属性时，都会执行一次搜索，目标是具有给定名字的属性。搜索首先从对象实例本身开始。如果在实例中找到该属性，则返回；如果没有找到，则继续搜索指针指向的原型对象。

```
f.name --> f.__proto__.name --> f.__proto__.__proto__.name ···
```

如果在实例中添加了一个属性，而该属性实例**原型**中的一个属性同名，那我们就在实例中创建该属性，该属性将会屏蔽原型中的那个属性。

## 原型链

每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向构造函数原型对象的内部指针。

让原型对象等于另一个类型的实例，此时，原型对象将包含一个指向另一个原型的指针，另一个原型的指针也包含着指向另一个构造函数的指针。如果另一个原型又是另外一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条。也就是**原型链**。


## Function、Object

```js
// true
Function.constructor === Function

// true
Function.prototype.constructor === Function

// true
Function.prototype.__proto__ === Object.prototype

// true
Object.constructor === Function

// true
Object.__proto__ === Function.prototype

```

## 图解

![](/assets/prototype.png)