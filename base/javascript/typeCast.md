# 类型转换

| 用例 | Number | String | Boolean |
| :--- | :---: | :---: | :---: |
| null |  |  | false |
| undefined |  |  | false |
| NaN |  |  | false |
| 0 |  |  | false |
| 1 |  |  | true |
| `""` |  |  | false |
| `"0"` |  |  | false |
| `"1"` |  |  | true |
| `"1aa"` |  |  | true |

## 数值转换



## 相等性判断

## `==`、`===` 和 `Object.is` 之间的区别

| 判断 | == | === | Object.is |
| :--- | :--- | :--- | :--- |
| `null` 与 `undefined` | true | false | false |
| `''` 等于 `0` | true | false | false |
| `''` 不等于 `'0'` | false | false | false |
| `'0'` 与 `0` | true | false | false |
| `'123'` 与 `123` | true | false | false |
| `+0` 与 `-0` | true | true | false |
| `NaN` 与 `NaN` | false | false | true |

非全等比较规则：

* 数字和字符串、布尔类型、数组进行比较时，非数字值先转为数字值，再进行比较
* 字符串和布尔值进行比较，进行比较的而两个数据同时转换为数字再比较
* `null` == `undefined`，`null` !== `undefined`
* 数组和字符串比较时，数组会转换成字符串再比较