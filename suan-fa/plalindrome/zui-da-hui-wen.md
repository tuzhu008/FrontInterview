## 最长回文

所谓最长回文子串问题，是指对于一个给定的母串，从所有的为回文串的子串中；找出最长的那一个。

## 算法

### 穷举法

```js
function isPalindromic(str) {
    let left = 0;
    let right = str.length - 1;
    while (left < right) {
        if (str[left++] !== str[right--]) {
            return false;
        }
    }

    return true;
}

function longestPalindrome (str) {
  let longestSub = '';

  for (let i = 0; i < str.length - 1; i++) {
    if (longestSub.length >= str.length - i) {
      break;
    }

    for (let j = str.length - 1; j >= i; j--) {
      const subStr = str.slice(i, j + 1);
      if (isPalindromic(subStr) && subStr.length > longestSub.length) {
        longestSub = subStr;
        break;
      }
    }
  }

  return longestSub;
}
```

三层嵌套循环，时间复杂度为 O\(n^3\)，空间复杂度为 O\(1\)

### 动态规划



