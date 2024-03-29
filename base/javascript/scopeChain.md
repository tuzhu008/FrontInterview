# 作用域链

## 执行环境

执行代码的环境称为**执行环境**。执行环境定义了变量或函数有权访问的其他数据，决定了他们各自的行为。

**执行环境的类型**只有两种：全局和局部（函数）。

**全局执行环境是最外围的一个执行环境。**

每个执行执行环境都有一个与之关联的**变量对象**，环境中定义的所有变量和函数都保存在这个对象中。

## 作用域链

当代码在一个环境中执行时，会创建变量对象的一个作用域链。  作用域链的用途，是保证对执行环境有权访问的所有变量和函数的有序访问。  
作用域链的前端，始终都是当前执行的代码所在环境的变量对象。  
如果这个环境是函数，则将其**活动对象**作为变量对象。  
活动对象在最开始只包含一个变量，即 `arguments` 对象。  
作用域链中的下一个变量对象来自包含环境（外部环境），而再下一个变量对象则来自于下一个包含环境。  
这样一直延续到全局执行环境；*全局执行环境的变量对象始终都是作用域链的最后一个对象*。

![](/assets/作用域链.png)

作用域链有时也可能会延长，有些语句在执行的时候会在作用域的前端临时增加一个环境变量对象，该变量对象会在代码执行后被移除。这样的语句有：

* try-catch 语句的 catch 块
* with 语句

标识符解析是沿着作用域链一级一级地搜索标识符的过程。  
搜索过程始终从作用域链的前端开始，然后逐级地向后回溯，直到找到标识符为止（如果找不到标识符，通常会导致报错）。

## 作用域链的定义

作用域链是指代码执行时，所在环境以及其包含环境所创建的变量对象组成的链式结构。  
用于保证代码对执行环境有权访问的所有变量和函数的有序访问。

此定义有几层意思

* 一个执行环境对应一个变量对象
* 变量对象包含该环境定义的变量和函数
* 从内向外的变量对象形成作用域链。
* 搜索作用域链是从内向外，不能从外向内。
* 作用域链本质上是一个指向变量对象的指针列表



