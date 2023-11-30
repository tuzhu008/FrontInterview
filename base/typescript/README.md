# Typesript


## 常见类型


## `type` 和 `interface` 的区别

* `interface` 可以重复声明，`type` 不行继承方式不一样
* `type` 使用交叉类型方式，`interface` 使用 `extends` 实现。

在对象扩展的情况下，使用接口继承要比交叉类型的性能更好。建议使用interface来描述对象对外暴露的借口，使用type将一组类型重命名（或对类型进行复杂编程）。

## any、unkonwn、never

// TODO:存异
any和unkonwn在TS类型中属于最顶层的Top Type，即所有的类型都是它俩的子类型。而never则相反，它作为Bottom Type是所有类型的子类型。

## 常见工具类型

* Partial
* Required
* Readonly
* Pick
* Omit
* Extract
* Exclude