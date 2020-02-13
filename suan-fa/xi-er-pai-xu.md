# 希尔排序算法

## 算法思想

* 假设第一个元素已排序
* 之后的元素当做插入新元素处理

## 算法动画演示

![](/assets/希尔排序.gif)

## 实现

### JavaScript

```js
function sort (arr) {
  let j;
  for (let gap = parseInt(arr.length / 2); gap > 0; gap = parseInt(gap / 2)) {
    for (let i = gap; i < arr.length; i++) {
      let tmp = arr[i];

      for (j = i; j >= gap && tmp < arr[j - gap]; j -= gap) {
        arr[j] = arr[j - gap];
      }
      arr[j] = tmp;
    }
  }

  return arr;
}
```



