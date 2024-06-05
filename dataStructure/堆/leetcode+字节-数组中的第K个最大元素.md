在未排序的数组中找到第 k 个最大的元素. 请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素. 

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4

说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度. 

[题目](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/)

```js
//  解法一: 数组排序，取第 k 个数
var findKthLargest = function(nums, k) {
  nums.sort((a, b) => b-a)

  return nums[k -1]
};

时间复杂度: O(nlogn)
空间复杂度: O(logn)

//  解法二: 构造前 k 个最大元素小顶堆，取堆顶
var findKthLargest = function(nums, k) {
  const heap = []

  for (let i=0; i<nums.length; i++) {
    if (heap.length < k) {
      heap.push(nums[i])

      if (heap.length === k) {
         buildMinHeap(heap);
      }
    } else {
      if (nums[i] > heap[0]) {
        heap[0] = nums[i]
        heapify(heap, k, 0)
      } 
    }
  }
  //  heap 是k 个最大元素小顶堆，堆顶是7个里面最小的，也就是第k个最大元素
  return heap[0]
}

const buildMinHeap = (heap) => {
  const len = heap.length
  for (let i=Math.floor(len / 2)-1; i>=0; i--) {
    heapify(heap, len, i)
  }
}

const heapify = (arr, len, i) => {
  let min = i
  const left = 2*i+1
  const right = 2*i+2

  if (left < len && arr[left] < arr[min]) {
    min = left;
  }

  // 如果右子节点存在且大于当前节点或左子节点，则更新最大值索引
  if (right < len && arr[right] < arr[min]) {
    min = right;
  }

  if (min !== i) {
    [arr[i], arr[min]] = [arr[min], arr[i]]
    heapify(arr, len, min)
  }
}

时间复杂度: 遍历数组需要 O(n) 的时间复杂度，一次堆化需要 O(logk) 时间复杂度，所以利用堆求 Top k 问题的时间复杂度为 O(nlogk)
空间复杂度: O(k)
```