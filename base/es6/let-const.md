## var let const 对比

| 对比 | var | let | const |
| :--- | :--- | :--- | :--- |
| 作用域 | 全局、局部 | 块级 | 块级 |
| 变量提升 | 是  | 否 | 否 |
| 可重复声明 | 是 | 否 | 否 |
| 可重新赋值 | 是 | 是 | 否 |
| 可不初始化| 是 | 是 | 否 |
| 暂时性死区 | 否 | 是 | 是 |

const实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动

## 初始化

  `var` 在声明时不进行初始化时，会自动使用 `undefined` 进行初始化

  `let` 在声明时不进行初始化时，不会自动初始化。

  `const` 在声明时不进行初始化程序会抛出错误。

## 暂时性死区

  先声明后使用。只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

  ```js
  let name = 'A';

  function getName() {
    console.log(name);
    let name = 'B'
  }

  // Uncaught ReferenceError: Cannot access 'name' before initialization
  getName();
  ```


