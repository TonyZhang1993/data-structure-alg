[题目](https://leetcode.cn/problems/top-k-frequent-elements/description/)

给定一个非空的整数数组,返回其中出现频率前 k 高的元素. 

```text
示例1
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]

示例2
输入: nums = [1], k = 1
输出: [1]
```

- 你可以假设给定的 k 总是合理的,且 1 ≤ k ≤ 数组中不相同的元素的个数. 
- 你的算法的时间复杂度必须优于 O(nlogn) , n 是数组的大小. 
- 题目数据保证答案唯一,换句话说,数组中前 k 个高频元素的集合是唯一的. 
- 你可以按任意顺序返回答案. 


## 解法一: map+数组

```js
let topKFrequent = function(nums, k) {
    //  map 记录每个数的频率, arr 记录去重后的数组
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
要求算法的时间复杂度必须优于 O(n log n) ,**所以这种实现不合题目要求**
```

## 解法二: map + 小顶堆
```js
let topKFrequent = function(nums, k) {
    //  map 记录每个数的频率, arr 记录去重后的数组
    let freq = new Map()

    nums.forEach((num) => {
        if (!freq.has(num)) {
            freq.set(num, 1)
        } else {
            freq.set(num, freq.get(num) + 1)
        }
    })

    let heap = [], len = 0
    freq.forEach((val, key) => {
        if (len < k) {
            heap.push(key)

            if (len === k-1) buildMinHeap(heap, freq, k)
        } else {
            if (val > freq.get(heap[0])) {
                heap[0] = key
                heaplify(heap, freq, k, 0)
            }
        } 
        len++
    })

    return heap
}
//  构建最小堆
/**
 * arr 堆数组
 * map 堆数组里每个元素对应的count 
 */
function buildMinHeap(heap, map, k) {
    for (let i = Math.floor(k / 2)-1; i>=0; i--) {
        heaplify(heap, map, k, i)
    }
}

function heaplify(heap, map, len, i) {
    let left = 2*i + 1
    let right = 2*i + 2
    let min = i

    if (left < len && map.get(heap[left]) < map.get(heap[min])) {
        min = left
    }
    if (right < len && map.get(heap[right]) < map.get(heap[min])) {
        min = right
    }

    if (min !== i) {
        [heap[i], heap[min]] = [heap[min], heap[i]]
        heaplify(heap, map, len, min)
    }
}
时间复杂度: 遍历数组需要 O(n) 的时间复杂度,一次堆化需要 O(logk) 时间复杂度,所以利用堆求 Top k 问题的时间复杂度为 O(nlogk)
空间复杂度: O(n)

tip: 
map.forEach((val, key) => {...}

```