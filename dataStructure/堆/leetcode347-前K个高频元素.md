https://leetcode.cn/problems/top-k-frequent-elements/description/

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

```text
示例1
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]

示例2
输入: nums = [1], k = 1
输出: [1]
```

- 你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
- 你的算法的时间复杂度必须优于 O(nlogn) , n 是数组的大小。
- 题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
- 你可以按任意顺序返回答案。


## 解法一：map+数组

```js
let topKFrequent = function(nums, k) {
    //  map 记录每个数的频率
    let map = new Map(), arr = [...new Set(nums)]

    nums.map((num) => {
        if (map.has(num)) {
            map.set(num, map.get(num) + 1)
        } else {
            map.set(num, 1)
        }
    })

    arr.sort((a, b) => {
        return map.get(b) - map.get(a)
    })

    return arr.slice(0, k)
}
O(n logn)
O(n)
要求算法的时间复杂度必须优于 O(n log n) ，**所以这种实现不合题目要求**
```

## 解法二：map + 小顶堆