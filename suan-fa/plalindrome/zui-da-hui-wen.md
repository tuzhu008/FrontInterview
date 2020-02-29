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
```





