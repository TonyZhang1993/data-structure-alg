[题目](https://leetcode.cn/problems/find-the-town-judge/description/)

小镇里有 n 个人，按从 1 到 n 的顺序编号. 传言称，这些人中有一个暗地里是小镇法官. 

如果小镇法官真的存在，那么: 

- 小镇法官不会信任任何人. 
- 每个人(除了小镇法官）都信任这位小镇法官. 
- 只有一个人同时满足属性 1 和属性 2 . 

给你一个数组 trust ，其中 trust[i] = [ai, bi] 表示编号为 ai 的人信任编号为 bi 的人. 

如果小镇法官存在并且可以确定他的身份，请返回该法官的编号; 否则，返回 -1 . 

示例 1: 
输入: n = 2, trust = [[1,2]]
输出: 2

示例 2: 
输入: n = 3, trust = [[1,3],[2,3]]
输出: 3

示例 3: 
输入: n = 3, trust = [[1,3],[2,3],[3,1]]
输出: -1

## 思路
本题是一道典型的有向图问题，不理解图的可以看这篇: 初学者应该了解的数据结构 Graph，寻找法官即是寻找 入度为N-1，出度为0的点

```js
let findJudge = function(N, trust) {
  //  创造N+1 是因为 题目的编号是从1-N
  let graph = Array.from({length: N+1}, () => ({outDegree: 0, inDegree: 0}))

  trust.forEach(([a, b]) => {
    graph[a].outDegree++
    graph[b].inDegree++
  })

  return graph.findIndex(({outDegree, inDegree}, index) => {
    return index !== 0 && outDegree === 0 && inDegree === N-1
  })
}

时间复杂度: O(N)
空间复杂度: O(N)
```


其实法官也是唯一一个 出入度差值为 N-1 ，所以这里可以简化为寻找出入度差值为 N-1

```js
let findJudge = function(N, trust) {
  let graph = Array(N+1).fill(0)

  trust.forEach(([a, b], index) => {
    graph[a]--
    graph[b]++
  })

  return graph.findIndex((item, index) => {
    return index !== 0 && item === N-1
  })
}

时间复杂度: O(N)
空间复杂度: O(N)
```

fill() 方法用一个固定值填充一个数组中从起始索引(默认为 0）到终止索引(默认为 array.length）内的全部元素. 它返回修改后的数组. 

```js
const array1 = [1, 2, 3, 4];

// Fill with 0 from position 2 until position 4
console.log(array1.fill(0, 2, 4));
// Expected output: Array [1, 2, 0, 0]

// Fill with 5 from position 1
console.log(array1.fill(5, 1));
// Expected output: Array [1, 5, 5, 5]

console.log(array1.fill(6));
// Expected output: Array [6, 6, 6, 6]

console.log(Array(3).fill(4)); // [4, 4, 4]

可以通过单个数字参数的构造函数创建数组，*数组的长度为传入的参数*，该数组不包含任何实际的元素. 
const arrayEmpty = new Array(2);

console.log(arrayEmpty.length); // 2
console.log(arrayEmpty[0]); // undefined; 实际上是一个空槽
console.log(0 in arrayEmpty); // false
console.log(1 in arrayEmpty); // false

const arrayOfOne = new Array("2"); // 这里是字符串 "2" 而不是数字 2

console.log(arrayOfOne.length); // 1
console.log(arrayOfOne[0]); // "2"

调用 Array() 时可以使用或不使用 new !!!!

```