## 题目 - TBD 

请用算法实现，从给定的无需、不重复的数组A中，取出N个数，使其相加和为M。并给出算法的时间、空间复杂度，如：
- review
```js
var arr = [1, 4, 7, 11, 9, 8, 10, 6];
var N = 3;
var M = 27;

Result:
[7, 11, 9], [11, 10, 6], [9, 8, 10]
```
## 思路分析
- 到这里其实我们就能发现一些规律，我们可以像`三数之和`那样，我们可以通过`大小指针`来逼近结果，从而达到降低一层时间复杂度的效果。
- 不管是几数之和，我们都用这种方法来进行优化。

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[][]}
 */
var nSum = function(nums, target) {
    const helper = (index, N, temp) => {
        // 如果下标越界了或者 N < 3 就没有必要在接着走下去了
        if (index === len || N < 3) {
            return
        }
        for (let i = index; i < len; i++) {
            // 剔除重复的元素
            if (i > index && nums[i] === nums[i - 1]) {
                continue
            }
            // 如果 N > 3 的话就接着递归
            // 并且在递归结束之后也不走下边的逻辑
            // 注意这里不能用 return
            // 否则循环便不能跑完整
            if (N > 3) {
                helper(i + 1, N - 1, [nums[i], ...temp])
                continue
            }
            // 当走到这里的时候，相当于在求「三数之和」了
            // temp 数组在这里只是把前面递归加入的数组算进来
            let left = i + 1
            let right = len - 1
            while (left < right) {
                let sum = nums[i] + nums[left] + nums[right] + temp.reduce((prev, curr) => prev + curr)
                if (sum === target) {
                    res.push([...temp, nums[i], nums[left], nums[right]])
                    while (left < right && nums[left] === nums[left + 1]) {
                        left++
                    }
                    while (left < right && nums[right] === nums[right - 1]) {
                        right--
                    }
                    left++
                    right--
                } else if (sum < target) {
                    left++
                } else {
                    right--
                }
            }
        }
    }
    let res = []
    let len = nums.length
    nums.sort((a, b) => a - b)
    helper(0, 4, [])  //  输入的case
    return res
};

```