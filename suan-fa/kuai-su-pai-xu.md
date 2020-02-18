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
  return quickSort(arr, 0, arr.length - 1);
}

function quickSort (arr, left, right) {
  if (left < right) {
    var pivot = media(arr, left, right);

    var i = left;
    var j = right - 1;

    while (i < j) {
      while (arr[++i] < pivot) {}
      while (arr[--j] > pivot) {}
      if (i < j) {
        swap(arr, i, j);
      }
    }

    swap(arr, i, right - 1);

    quickSort(arr, left, i - 1);
    quickSort(arr, i + 1, right);
  }

  return arr;
}

function media (arr, left, right) {

  var center = parseInt((left + right) / 2);

  if (arr[center] < arr[left]) {
    swap(arr, left, center);
  }

  if (arr[right] < arr[left]) {
    swap(arr, left, right);
  }

  if (arr[right] < arr[center]) {
    swap(arr, center, right);
  }

  swap(arr, center, right - 1);
  return arr[right - 1];
}

function swap (arr, i, j) {
  var tmp = arr[i];
  arr[i] = arr[j];
  arr[j] = tmp;
}
```

## 优化

快速排序在针对少量元素的数组时效率不佳，因此可以考虑在数组元素过少时采用其他算法。

```js
const CUTOFF = 20;

function quickSort (arr, left, right) {
  if (left + CUTOFF < right) {
    var pivot = media(arr, left, right);

    var i = left;
    var j = right - 1;

    while (i < j) {
      while (arr[++i] < pivot) {}
      while (arr[--j] > pivot) {}
      if (i < j) {
        swap(arr, i, j);
      }
    }

    swap(arr, i, right - 1);

    quickSort(arr, left, i - 1);
    quickSort(arr, i + 1, right);
  } else {
    insertSort(arr);
  }

  return arr;
}
```



