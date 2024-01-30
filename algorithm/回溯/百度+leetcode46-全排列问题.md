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


//  method2 - dfs
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