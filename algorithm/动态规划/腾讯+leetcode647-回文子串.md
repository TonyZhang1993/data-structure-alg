[题目](https://leetcode.cn/problems/palindromic-substrings/description/)

- 参考最长回文子串

转换一下,统计个数即可

```JS
var countSubstrings = function(s) {
  let res = 0

  for (let i=0; i<s.length; i++) {
    // 以 s[i] 为中心的最长回文子串 - 奇数情况
    let s1 = palindrome(s, i, i)
    // 以 s[i] 和 s[i+1] 为中心的最长回文子串 - 偶数情况
    let s2 = palindrome(s, i, i+1)

    res += s1
    res += s2

  }

  return res
  // 在 s 中寻找以 s[l] 和 s[r] 为中心的最长回文串
  function palindrome(s, l, r) {
    let res = 0
    //  边界情况
    while (l>=0 && r<s.length && s[l] === s[r]) {
      //  向两边展开
      l--
      r++
      res++
    }

    // return s.slice(l+1, r)
    return res
  }
}

//  时间复杂度: O(n2)
//  空间复杂度: O(1)
```