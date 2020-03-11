# 继承

## 原型链继承

```js
function SuperType () {
  this.property = true;
}

SuperType.prototype.getSuperValue = function () {
  return this.property;
}

function SubType () {
  this.subProperty = false;
}

SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function () {
  return this.subProperty;
}
```

**缺点：** 实例共享了原型链上其他类型实例的属性。因为这些类型的实例属性现在成了实例的原型属性。

## 借用构造函数

```js
function SuperType () {
  this.colors = ['red', 'green'];
}

function SubType () {
  SuperType.call(this);
}
```

**缺点：**

* 方法都得在超类的内部定义，每个实例都会有一个不同的方法实例，无法共享（在创建对象的时候，构造函数的缺点一样）。
* 超类的原型方法不可见

## 组合继承

将原型链继承和借用构造函数的技术组合到一起。

```js
function SuperType (name) {
  this.name = name;
  this.colors = ['red', 'green'];
}

SuperType.prototype.sayName = function () {
  alert(this.name);
}

function SubType (name, age) {
  SuperType.call(this, name);
  this.age = age;
}

SubType.prototype = new SuperType();
SubType.prototype.sayAge = function () {
  alert(this.age);
}
```

**缺点：**不论什么时候都会调用两次超类的构造函数

## 原型式继承

```js
function object (o) {
  function F () {}
  F.prototype = o;
  return new F();
}
```

ES6 中引入了 `Object.create()` 以达到相同目的。

可以应用到只需要轻量继承的情况下。

**缺点：** 原型属性在实例间共享的问题

## 寄生式继承

```js
function createAnother (original) {
  var clone = object(original);
  clone.sayHi = function () {
    alert('Hi');
  }

  rturn clone;
}
```

`object` 可以是任何能够返回新对象的函数。

## 寄生组合式继承

思路：不必为了指定类型的原型而调用超类型的构造函数，我们需要的无非是超类原型的一个副本。

```js
function inhertPrototype (subType, superType) {
  var prototype = object(superType.prototype);
  prototype.constructor = subType;
  subType.prototype = prototype;
}

function SuperType (name) {
  this.name = name;
  this.colors = ['red', 'green'];
}

SuperType.prototype.sayName = function () {
  alert(this.name);
}

function SubType (name, age) {
  SuperType.call(this, name);
  this.age = age;
}

inhertPrototype(SubType, SuperType);

SubType.prototype.sayAge = function () {
  alert(this.age);
}
```



