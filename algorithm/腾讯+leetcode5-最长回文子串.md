
下面就借最长回文子序列这个问题，详解一下第二种情况下如何使用动态规划。

[最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/description/)

[双指针技巧](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/#%E4%B8%80%E3%80%81%E5%BF%AB%E6%85%A2%E6%8C%87%E9%92%88%E6%8A%80%E5%B7%A7)

题解
```js
//  寻找回文串的问题核心思想是：从中间开始向两边扩散来判断回文串，对于最长回文子串，就是这个意思：
var longestPalindrome = function(s) {
  let res = ''

  for (let i=0; i<s.length; i++) {
    // 以 s[i] 为中心的最长回文子串 - 奇数情况
    let s1 = palindrome(s, i, i)
    // 以 s[i] 和 s[i+1] 为中心的最长回文子串 - 偶数情况
    let s2 = palindrome(s, i, i+1)

    res = res.length > s1.length ? res : s1
    res = res.length > s2.length ? res : s2

  }

  return res

  function palindrome(s, l, r) {
    //  边界情况
    while (l>=0 && r<s.length && s[l] === s[r]) {
      //  向两边展开
      l--
      r++
    }

    return s.slice(l+1, r)
  }
}


```
