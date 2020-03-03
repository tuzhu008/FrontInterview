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

### 最长公共子串

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

### 中心扩展算法

中心有两种：

* 单个元素为中心，向两边扩展。得到的结果为基数长度。
* 相邻元素的中间为中心，向两边扩展。也可以认为是相邻元素组成的集合为中心。得到的结果为偶数长度。

```js
function longestPalindrome (s) {
  if (s == null || s === '') {
    return '';
  }

  let start = 0;
  let end = 0;

  for (let i = 0; i < s.length; i++) {
    const len1 = expandAroundCenter(s, i, i);
    const len2 = expandAroundCenter(s, i, i + 1);
    const len = Math.max(len1, len2);
    if (len > end - start) {
      start = i - parseInt((len - 1) / 2); // 减去 i 本身
      end = i + parseInt(len / 2);
    }
  }

  return s.substring(start, end + 1); // 由于 substring 是一个半闭半开区间，所以加 1
}

function expandAroundCenter (s, left, right) {
  let L = left;
  let R = right;

  while (L >=0 && R < s.length && s[L] === s[R]) {
    L--;
    R++;
  }

  return R - L - 1;
}
```

### Manachers 算法

个人认为，该算法是

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

  let strArr = sb.split('');
  // 回文半径数组
  let radiusArr = new Array(strArr.length);

  // 右边界
  let max = 0;
  // 右边界索引
  let maxIndex = 0;

  // 最大回文半径
  let maxRadius = 0;
  let maxRadiusIndex = 0;

  for (let i = 1; i < strArr.length; i++) {
    // 2 * maxIndex - i 找到一个 i 在 maxIndex 左边的对称点 i‘
    // i' 的 radius 与 max - i 的大小不知，因此将其置为其中的较小值
    // 这个最小值也许不能满足当前 i 的回文规则，
    radiusArr[i] = max > i
      ? Math.min(radiusArr[2 * maxIndex - i], max - i) : 1;

    while (i - radiusArr[i] >= 0 &&
      i + radiusArr[i] < strArr.length &&
      strArr[i - radiusArr[i]] === strArr[i + radiusArr[i]]) {
      radiusArr[i]++;
    }

    if (i + radiusArr[i] > max) {
      max = i + radiusArr[i];
      maxIndex = i;
    }

    if (maxRadius < radiusArr[i]) {
      maxRadius = radiusArr[i];
      maxRadiusIndex = i;
    }
  }
  let maxStart = (maxRadiusIndex - maxRadius + 1) / 2;
  return s.substring(maxStart, maxStart + maxRadius - 1);
}
```

## 参考

* [Leetcode](https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zui-chang-hui-wen-zi-chuan-by-leetcode/)



