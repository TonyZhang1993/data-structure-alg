[leetcode151](https://leetcode.cn/problems/reverse-words-in-a-string/description/)

使用API
```js
var reverseWords = function(s) {
    return s.trim().split(/\s+/).reverse().join(' ')
};
```

双端队列

- 首先去除字符串左右空格
- 逐个读取字符串中的每个单词,依次放入双端队列的对头
- 再将队列转换成字符串输出(已空格为分隔符)

```js
var reverseWords = function(s) {
  let left = 0, right = s.length-1
  let queue = [], word = ''

  while(s[left] === ' ') left++
  while(s[right] === ' ') right--

  while(left<=right) {
    if (s[left] === ' ' && word) {
      queue.unshift(word)
      word = ''
    } else if (s[left] !== ' ') {
      word += s[left]
    }
    left++
  }

  queue.unshift(word)

  return queue.join(' ')
};
```