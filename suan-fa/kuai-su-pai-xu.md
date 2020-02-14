# 快速排序算法

## 算法思想

* 设置一个标志位，遍历数组，将大小元素分置于标志位两侧。
* 对标志位两侧子数组进行快速排序迭代，直到子数组为空。

## 算法动画演示

![](/assets/快速排序.gif)

## 实现

### JavaScript

```js
function sort (arr) {
  if (arr.length > 1) {
    const smaller = [];
    const same = [];
    const larger = [];

    const chooseItem = arr[parseInt(arr.length / 2)];

    for (i of arr) {
      if (i < chooseItem) {
        smaller.push(i);
      } else if (i > chooseItem) {
        larger.push(i);
      } else {
        same.push(i);
      }
    }

    arr = [].concat(sort(smaller), same, sort(larger));
  }

  return arr;
}
```



