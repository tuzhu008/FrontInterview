# 快速排序算法

## 算法思想

* ## 算法动画演示

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



