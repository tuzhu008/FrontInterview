# 创建对象

## 工厂模式

```js
function createPerson (name, age, job) {
  var o = new Object();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function () {
    alert(this.name);
  };
  return o;
}
```

**缺点：**工厂模式解决了创建相似对象的问题，但未解决对象识别的问题（即怎么知道对象的类型）。

## 构造函数模式

```js
function Person (name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function () {
    alert(this.name);
  };
}

var person1 = new Person('wahah', 18, 'student');
```

使用 `new` 关键字创建对象，实际经历了以下四个步骤：

* 创建一个新对象
* 将构造函数的作用域赋给新对象（因此 `this` 就指向了这个新对象）
* 执行构造函数中的代码
* 返回新对象

创建自定义的构造函数意味着可以将它的实例标识为一种特定的类型。

**缺点：**使用构造函数的主要问题在于，每个方法都要在每个对象上重新定义一遍。

## 原型方式

我们创建的每一个函数都有一个 `prototype` 属性，这个属性是一个指针，指向一个对象，而这个对象的咏于是包含可以由也定累心的所有实例共享的属性和方法。

```js
function Person () {}

Person.prototype.name = 'wahaha';
Person.prototype.age = 18;
Person.prototype.job = 'student';
Person.prototype.sayName = function () {
  alert(this.name);
};
```

为了从视觉上更好的封装原型对象，更常见的做法是用一个包含所有属性和方法的对象字面量来重写整个原型对象：

```js
function Person () {}

Person.prototype = {
  name: 'wahaha',
  age: 18,
  job: 'student',
  sayName: function () {
    alert(this.name);
  }
};
```

**缺点：** 

* 默认情况下，所有实例都将取得相同的属性值
* 原型中的所有属性共享，特别是某个原型属性是引用类型

## 组合使用构造函数模式和原型模式

```js
function Person (name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = ['lili', 'lucy'];
}

Person.prototype = {
  constructor: Person,
  sayName: function () {
    alert(this.name);
  }
};
```

## 动态原型模式

```js
function Person (name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = ['lili', 'lucy'];

  if (typeof this.sayName !== 'function') {
    Person.prototype.sayName = function () {
      alert(this.name);
    }
  }
}
```

## 寄生构造函数模式

```js
function Person (name, age, job) {
  var o = new Object();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function () {
    alert(this.name);
  };
  return o;
}
```

这个模式可以用来在特殊情况下为对象创建构造函数。

如，想创建一个具有额外方法的特殊数组，由于不能直接修改 Array 构造函数：

```js
function SpecialArray () {
  var values = new Array();

  // 添加值
  values.push.apply(values, arguments);

  // 添加方法
  values.toPipedString = function () {
    return this.join('|');
  }

  return values;
}
```

**缺点：**无法使用 `instanceof` 确定对象的类型

## 稳妥构造函数模式

不适用 `new` 和 `this` 的寄生模式。

```js
function Person (name, age, job) {
  var o = new Object();

  o.sayName = function () {
    alert(name);
  }
}
```