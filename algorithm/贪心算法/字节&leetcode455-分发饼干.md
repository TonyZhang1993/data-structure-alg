题目：

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。


获取数组最后一位方法

```js
let a = [1,2,3,4]

a[a.length-1]

a.slice(-1)[0]

a.pop()

a.at(-1)
```

```js
var findContentChildren = function(g, s) {
  g.sort((a, b) => a - b)
  s.sort((a, b) => a - b)

  let res = 0
  let index = 0

  for (let i=0; i<g.length; i++) {

    if (g[i]>s.at(-1)) break

    for (let j=index; j<s.length; j++) {

      if (g[i] <= s[j]) {
        res++
        index=j+1
        break
      }
    }

  }

  return res

};
```