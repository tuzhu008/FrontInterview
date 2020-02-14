# 归并排序算法

## 算法思想

利用迭代思想将复杂的数组切割为单元素数组，并且从底层开始对两个数组进行排序合并。

## 算法动画演示

![](/assets/归并排序.gif)

## 实现

### JavaScript

```js
function sort (arr) {
  if (arr.length < 2) {
    return arr;
  }
  const middle = parseInt(arr.length / 2);
  const left = arr.slice(0, middle);
  const right = arr.slice(middle);

  return merge(sort(left), sort(right));
}

function merge (left, right) {
  let result = [];
  let i = 0;

  while (left.length > 0 && right.length > 0) {
    if (left[0] <= right[0]) {
      result[i++] = left.shift();
    } else {
      result[i++] = right.shift();
    }
  }

  while (left.length > 0) {
    result[i++] = left.shift();
  }

  while (right.length > 0) {
    result[i++] = right.shift();
  }

  return result;
}
```



