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

  for (let i = 0; i < str.length; i++) {
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

#### 思路

若长度为 L 的字符串为回文，则去掉首尾的字符，长度为 L-2 的字符串也为回文。  
即全局最优解包含局部最优解。

#### 最小子问题

1. 单个字符独立成为一个回文字符串
2. 相邻的两个相同字符，是一个回文字符串

#### 递推方程

设置一个 L\*L 的矩阵 D，D\[i\]\[j\] 的值为 ture 或 false， 表示从 i 起始 j 终止的字符串是否为回文。

则有：

> D\[i\]\[j\] = \(D\[i\] === D\[j\]\) && D\[i+1\]\[j-1\]
>
> （若第 i 个字符与第 j 个字符相同，且从 i+1 起始 j-1 终止的字符串为回文，则有从 i 起始 j 终止的字符串也为回文）

```js
function longestPalindrome (str) {
  const L = str.length;
  const d = [];

  for (let i = 0; i < L; i++) { // 初始化矩阵D，且先将最小子问题 1 的情况都设置为 true
    let arr = [];
    arr[i] = true;
    d[i] = arr;
  }


  for (let i = 0; i < L; i++) { // 再将最小子问题 2 的情况都设置为true
    if (str[i] === str[i + 1]) {
      d[i][i + 1] = d[i+1][i] = true;
    }
  }

  let maxLen = 1;
  let resIndex = 0;


  for (let len = 2; len <= L; len++) { // len 为回文子串长度
    for (let i = 0; i < L - len + 1; i++) { // 从第0个位置开始，依据最小子问题1、2来依次检查回文字符串
      const lastIndex = i + len - 1;

      if (str[i] === str[lastIndex] && d[i + 1][lastIndex - 1]) {
        d[i][lastIndex] = true;

        if (len > maxLen) { // 满足条件时，保存回文字符串长度和起始位置
          maxLen = len;
          resIndex = i;
        }
      }
    }
  }

  return str.substr(resIndex, maxLen)
}
```

### 

### 中心扩展算法

### Manachers算法

```js
function longestPalindrome (s) {
  if (s == null || s.length <= 1) {
    return s;
  }

  let sb = '$#';

  for (let i = 0; i < s.length; i++) {
    sb += s[i];
    sb += '#';
  }

  let str = sb.split('');
  let r = new Array(str.length);

  let mx = 0;
  let id = 0;
  let ansR = 0;
  let ansCenter = 0;

  for (let i = 1; i < str.length; i++) {
    r[i] = mx - i > r[i] ? Math.min(r[2 * id - i], mx - i) : 1;
    while (i - r[i] >= 0 && i + r[i] < str.length && str[i - r[i]] == str[i + r[i]]) {
      r[i]++;
    }

    if (i + r[i] > mx) {
      mx = i + r[i];
      id = i;
    }
    if (ansR < r[i]) {
      ansR = r[i];
      ansCenter = i;
    }
  }
  let maxStart = (ansCenter - ansR + 1) / 2;
  return s.substring(maxStart, maxStart + ansR - 1);
}
```



