## 题目

给定一个数组 nums,有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧. 你只可以看到在滑动窗口 k 内的数字. 滑动窗口每次只向右移动一位.  返回滑动窗口最大值. 

```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 
  滑动窗口的位置               最大值
---------------              -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7

```

```js
//  暴力解法
var maxSlidingWindow = function (nums, k) {
  if (nums === 1) return nums

  let arr = [], result = []
  for (let i=0; i<nums.length ;i++) {
    arr.push(nums[i])

    if (i >= k-1) {
      result.push(Math.max(...arr))
      arr.shift()
    }
  }

  return result
};

O(n*k)
//  在每次循环中,使用 Math.max(...arr) 查找滑动窗口中的最大值的时间复杂度为 O(k). 
O(n)

//  双端队列
var maxSlidingWindow = function (nums, k) {
  const deque = []  //  存放双端队列 - 两边可出可进;  存储滑动窗口中的元素的索引
  const result = [] //  保存滑动窗口中的最大值

  for (let i=0; i<nums.length; i++) {
    // 如果队列的第一个索引不在当前滑动窗口内,将其从队列中移除
    if (i - deque[0] >= k) {
      deque.shift()
    }
    // 从队列末尾开始,移除所有小于等于当前元素值的索引
    while(nums[deque[deque.length - 1]] <= nums[i]) {
      deque.pop()
    }
    // 将当前元素的索引加入队列
    deque.push(nums[i])
    // 当遍历到滑动窗口的最后一个元素时,将队列的第一个索引对应的元素(即滑动窗口的最大值)加入结果数组
    if (i >= k-1) {
      result.push(nums[deque[0]])
    }
  }

  return result
};

O(n)
O(n)
```