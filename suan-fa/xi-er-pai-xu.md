# 希尔排序算法

## 算法思想

* 使用增量序列进行排序

## 算法动画演示

![](/assets/希尔排序.gif)

## 实现

### JavaScript

```js
function sort (arr) {
  const getGap = (value) => parseInt(value / 2);

  for (let gap = getGap(arr.length); gap > 0; gap = getGap(gap)) {
    for (let i = gap, len = arr.length; i < len; i++) {
      const tmp = arr[i];
      let j = i;

      while (j > gap && tmp < arr[j - gap]) {
        arr[j] = arr[j - gap];
        j -= gap;
      }

      arr[j] = tmp;
    }
  }

  return arr;
}
```

* 第一层循环用来控制增量序列
* 第二层循环用来控制插入的元素
* 第三层循环用来查找增量插入的位置



