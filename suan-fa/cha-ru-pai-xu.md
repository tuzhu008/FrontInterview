# 插入排序算法

## 算法动画演示

![](/assets/插入排序.gif)

## 实现

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



