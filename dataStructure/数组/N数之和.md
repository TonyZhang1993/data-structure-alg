## 题目

请用算法实现,从给定的无序、不重复的数组A中,取出N个数,使其相加和为M. 并给出算法的时间、空间复杂度,如: 
- review
```js
var arr = [1, 4, 7, 11, 9, 8, 10, 6];
var N = 3;
var M = 27;

Result:
[7, 11, 9], [11, 10, 6], [9, 8, 10]
```
## 思路分析
- 到这里其实我们就能发现一些规律,我们可以像`三数之和`那样,我们可以通过`大小指针`来逼近结果,从而达到降低一层时间复杂度的效果. 
- 不管是几数之和,我们都用这种方法来进行优化. 

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
            // 当走到这里的时候,相当于在求「三数之和」了
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

## 方法二
要从给定的无序、不重复的数组 A 中取出 N 个数,使其相加和为 M,可以使用回溯算法(Backtracking)来解决这个问题. 

回溯算法的基本思想是通过深度优先搜索的方式遍历所有可能的组合,并在搜索过程中进行剪枝,提高效率. 


```js

/**
 * nums - 当前 目标数组
 * target - 当前 目标和
 * N - 剩余 取出数的个数
 * path - 当前路径数组
 * result - 
 */
function backtrack(nums, target, N, path, result) {
    //  终止条件判定
    if (N === 0 && target === 0) {
        result.push([...path])
        return
    }
    //  不满足条件
    if (N <= 0) {
        return 
    }
    //  不能用 for (let i in nums)
  for (let i = 0; i < nums.length; i++) {
    //  当前元素加入路径
    path.push(nums[i]);
    backtrack(nums.slice(i + 1), target - nums[i], N - 1, path, result);
    //  当前元素弹出路径,对后面的元素进行循环
    path.pop();
  }
}

/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[][]}
 */
var fourSum = function(nums, target) {
    const result = []
    nums.sort((a, b) => a - b) // 对输入数组进行排序
    backtrack(nums, target, 4, [], result)
    return result
};

const backtrack = (nums, target, N, path, result) => {
    //  终止条件判定
    if (N === 0 && target === 0) {
        result.push([...path])
        return
    }
    // 添加剪枝条件
    if (N <= 0 || nums.length < N || nums[0] * N > target || nums[nums.length-1] * N < target) {
        return 
    }

    //  不能用 for (let i in nums)
    for (let i = 0; i < nums.length; i++) {
         // 跳过相同的元素
        if (i > 0 && nums[i] === nums[i-1]) continue
        //  当前元素加入路径
        path.push(nums[i])
        backtrack(nums.slice(i+1), target - nums[i], N - 1, path, result)
        //  当前元素弹出路径,对后面的元素进行循环
        path.pop()
    }
}



//  1 去重, 2 保证同一解答的唯一性
为了保证解的唯一性和去重,可以在 backtrack 函数中对选择进行排序,并在递归调用时跳过相同的元素. 这样可以确保每个解只被添加一次,并且不会重复. 

```

tip: 
数组是引用类型,函数中传递的参数是实参的地址,而不是值本身,所以在函数中对参数进行操作时,实际上是修改了原始数组的内容. 

在 JavaScript 中,`基本数据类型(如数字、布尔值、字符串等)`是`按值传递`的,而`对象(包括数组、函数等)`是按`引用传递`的. 

for ... in是为遍历对象属性而构建的,不建议与数组一起使用,

for...in 语句以任意顺序迭代一个对象的除Symbol以外的可枚举属性,包括`继承的可枚举属性`. 

数组可以用`Array.prototype.forEach()`和`for ... of`

forEach(item, index, array); 需要注意的是 forEach() 不会在迭代之前创建数组的副本. forEach 过程中如果有对数组的更改,则直接影响