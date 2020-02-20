# 桶排序算法

## 适用范围

输入数据 A1，A2，···，An 必须仅由小于 M 的正整数组成。

## 算法思想

## 算法动画演示

## 实现

### JavaScript

```js
function sort (arr) {
  for (let i = 1; i < arr.length; i++) {
    let curr = arr[i];
    let j = i;

    for (j; curr < arr[j - 1]; j--) {
      arr[j] = arr[j - 1];
    }

    arr[j] = curr;
  }

  return arr;
}
```



