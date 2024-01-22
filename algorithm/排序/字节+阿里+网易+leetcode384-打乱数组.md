// 以数字集合 1, 2 和 3 初始化数组。
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// 打乱数组 [1,2,3] 并返回结果。任何 [1,2,3]的排列返回的概率应该相同。
solution.shuffle();

// 重设数组到它的初始状态[1,2,3]。
solution.reset();

// 随机返回数组[1,2,3]打乱后的结果。
solution.shuffle();

打乱一个没有重复元素的数组。

https://leetcode.cn/problems/shuffle-an-array/description/

```js
/**
 * @param {number[]} nums
 */
var Solution = function(nums) {
    this.nums = nums
};

/**
 * @return {number[]}
 */
Solution.prototype.reset = function() {
    return this.nums
};

/**
 * @return {number[]}
 */
Solution.prototype.shuffle = function() {
    let result = this.nums.slice();

    const len = result.length;

    for(let i=len-1; i>=0; i--) { //  从0 开始也ok
        let target = Math.floor((i+1) * Math.random());
        [result[i], result[target]] = [result[target], result[i]];
    }
    return result;
};
```

