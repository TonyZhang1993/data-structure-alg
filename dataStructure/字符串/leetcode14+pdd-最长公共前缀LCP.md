[leetcode14](https://leetcode.cn/problems/longest-common-prefix/description/)

```js
//  逐个比较
var longestCommonPrefix = function(strs) {
  if (strs === null || strs.length === 0) return ''
  if (strs.length === 1) return strs[0]

  let prev = strs[0]

  for (let i=1; i<strs.length; i++) {
    let j = 0

    for (;j<strs[i].length && j < pre.length;j++) {
      if (prev.charAt(j) !== strs[i].charAt(j)) break
    }
    prev = prev.slice(0, j)
    if (prev === '') return ''
  }

  return prev
}

//  拿首个,进行比较,不断缩减长度查看
var longestCommonPrefix = function(strs) {
    let prev = strs[0]
    if (strs.length === 1) return strs[0]

    for (let i=1; i<strs.length; i++) {
        while(!strs[i].startsWith(prev)) {
            prev = prev.slice(0, prev.length - 1)
        }
    }
    return prev
}
```