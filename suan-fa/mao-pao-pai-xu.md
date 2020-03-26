# 冒泡排序算法

## 算法思想

## 算法动画演示

![](/assets/冒泡排序.jpeg)

## 实现

### JavaScript

```js
function sort (arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0, len = arr.length - 1 - i; j < len; j++) {
      if (arr[j + 1] < arr[j]) {
        let temp = arr[j + 1];
        arr[j + 1] = arr[j];
        arr[j] = temp;
      }
    }
  }

  return arr;
}
```



