# 选择排序算法

## 算法思想

## 算法动画演示

![](/assets/选择排序.jpeg)

## 实现

### JavaScript

```js
function sort (arr) {
  for (let i = 1, len = arr.length; i < len; i++) {
    const tmp = arr[i];
    let j = i;

    while (j > 0 && tmp < arr[j - 1]) {
      arr[j] = arr[j - 1];
      j--;
    }

    if (i !== j) {
      arr[j] = tmp;
    }
  }

  return arr;
}
```



