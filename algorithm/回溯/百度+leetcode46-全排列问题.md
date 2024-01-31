## 全排列 I

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
附赠leetcode地址：https://leetcode.cn/problems/permutations/description/

```js
//  method1 - swap TBD
function permute(nums) {
  let res = []

  const backtrack = (startIndex, nums) => {
    // 当前排列已经完成，将结果加入到结果数组中
    if (startIndex === nums.length - 1) {
      res.push([...nums])
      return
    }

    for (let i=startIndex; i<nums.length; i++) {
      // 将当前元素与后面的元素依次交换位置，生成新的排列
      [nums[startIndex], nums[i]] = [nums[i], nums[startIndex]];
      // 递归调用，生成下一个位置的排列
      backtrack(startIndex+1, nums);
      // 恢复原数组，进行回溯
      [nums[startIndex], nums[i]] = [nums[i], nums[startIndex]]

    }
  }

  backtrack(0, nums)
  return res
}


//  method2 - dfs [适用于没有重复的字符]
function permute(nums) {
  let res = []

  function dfs(track) {
    if (track.length === nums.length) {
      res.push([...track])
      return
    }

    for (let num of nums) {
      //  剪枝操作
      if (track.includes(num)) {
        continue
      }

      track.push(num)
      dfs(track)
      track.pop() //  回溯操作
    }
  }

  dfs([])

  return res
}
```


## 全排列 II

给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

```js
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

```js
function permuteUnique(nums) {
  const result = []
  nums.sort((a, b) => a - b)  //  排序，为了后面剪枝方便

  function backtrack(cur, visited) {
    if (cur.length === nums.length) {
      result.push(cur.slice())
      return
    }

    for (let i=0; i<nums.length; i++) {
      // 已经访问过或者与前一个数字相同且前一个数字未被访问，则跳过
      if (visited[i] || (i>0 && nums[i] === nums[i-1] && !visited[i-1])) {
        continue
      }

      visited[i] = true // 标记当前数字已访问
      cur.push(nums[i]) // 加入当前数字
      backtrack(cur, visited) // 递归处理下一个位置
      cur.pop() // 回溯
      visited[i] = false  // 取消标记
    }
  }
  let visited = new Array(nums.length).fill(false)
  backtrack([], visited)
  return result
}
```

tip: 剪枝条件的理解
```js
if (vis[i] || (i > 0 && nums[i] == nums[i - 1] && !vis[i - 1])) {
      continue;
}

举个栗子，对于两个相同的数11，我们将其命名为1a1b, 1a表示第一个1，1b表示第二个1； 那么，不做去重的话，会有两种重复排列 1a1b, 1b1a， 我们只需要取其中任意一种排列； 为了达到这个目的，限制一下1a, 1b访问顺序即可。 比如我们只取1a1b那个排列的话，只有当visit nums[i-1]之后我们才去visit nums[i]， 也就是如果!visited[i-1]的话则continue
```