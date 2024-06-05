[题目](https://leetcode.cn/problems/valid-triangle-number/description/)

给定一个包含非负整数的数组 nums ,返回其中可以组成三角形三条边的三元组个数. 

- 解法: 排序+双指针

背景: 三角形的任意两边之和大于第三边,任意两边之差小于第三边,如果这三条边长从小到大为 a ,  b ,  c ,当且仅当 a + b > c 这三条边能组成三角形

- 解题思路:  先数组排序,排序完后,固定最长的边,利用双指针法判断其余边


以 `nums[nums.length - 1]` 作为最长的边 `nums[k] ( k = nums.length - 1 )`

以 `nums[i]` 作为最短边,以 `nums[nums.length - 2]` 作为第二个数 `nums[j] ( j = nums.length - 2 )` ,

判断 `nums[i] + nums[j]` 是否大于 `nums[k]` ,
`
`nums[i] + nums[j] > nums[k]` ,则: 

`nums[i+1] + nums[j] > nums[k]
nums[i+2] + nums[j] > nums[k]
...
nums[j-1] + nums[j] > nums[k]`
则可构成三角形的三元组个数加 `j-i` ,并且 j 往前移动一位( j-- ), 继续进入下一轮判断

`nums[i] + nums[j] <= nums[k]`,则 i 往后移动一位(nums 是增序排列),继续判断


```js

/**
 * @param {number[]} nums
 * @return {number}
 */
var triangleNumber = function(nums) {
  if (!nums || nums.length < 3) return 0

  let count = 0
  //  排序
  nums.sort((a, b) => a - b)

  for (let k=nums.length - 1; k>1; k--) {
    //  最小数
    let i=0
    //  当前最大数的前一位
    let j = k-1

    while(i<j) {
      if (nums[i] + nums[j] > nums[k]) {
        //  因为num[j] + num[i]足够大了,所以从 num[i] 到 num[j-1] 的数字 加上 num[j] 都可以符合条件
        count += j - i
        j--
      } else {
        i++
      }
    }
  }

  return count
};

O(n^2)
O(1)


tip: 
Array.prototype.sort()

在 V8 引擎 7.0 版本之后就舍弃了快速排序,因为它不是稳定的排序算法,在最坏情况下,时间复杂度会降级到 O(n2)

而是采用了一种混合排序的算法: TimSort 

在数据量小的子数组中使用插入排序,然后再使用归并排序将有序的子数组进行合并排序,时间复杂度为 O(nlogn)
```