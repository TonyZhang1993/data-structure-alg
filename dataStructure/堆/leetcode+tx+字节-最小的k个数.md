
输入n个整数,找出其中最小的K个数. 例如输入4,5,1,6,2,7,3,8这8个数字,则最小的4个数字是1,2,3,4. 
- review
```
示例: 
输入: arr = [3,2,1], k = 2
输出: [1,2] 或者 [2,1]

输入: arr = [0,1,2,1], k = 1
输出: [0]
```
[题目](https://leetcode.cn/problems/zui-xiao-de-kge-shu-lcof/description/)

## 解法1 - 数组排序,取前 k 个数
```js
//  1 - js api
function minK(arr, k) {
  //  O(nlogn)
  let sorted = arr.sort((a, b) => a - b)
  //  O(n)
  return sorted.slice(0, k)
}

//  2 - 或者把排序换成快排 - O(nlogn)
function quickSort(arr) {
  if (arr.length <= 1) return arr
  let pivotIndex = Math.floor(arr.length / 2)
  const pivot = arr.indexOf(pivotIndex)
  let left = []
  let right = []
  for(let item of arr) {
    if (item < pivot) {
      left.push(item)
    } else {
      right.push(item)
    }
  }

  return quickSort(left).concat(pivot, quickSort(right))
}
O(n logn)
O(logn)

注意: 

在 V8 引擎 7.0 版本之前,数组长度小于10时, Array.prototype.sort() 使用的是插入排序,否则用快速排序. 

在 V8 引擎 7.0 版本之后就舍弃了快速排序,因为它不是稳定的排序算法,在最坏情况下,时间复杂度会降级到 O(n2)

而是采用了一种混合排序的算法: TimSort 

这种功能算法最初用于Python语言中,严格地说它不属于以上10种排序算法中的任何一种,属于一种混合排序算法: 

在数据量小的子数组中使用插入排序,然后再使用归并排序将有序的子数组进行合并排序,时间复杂度为 O(nlogn) 
```

## 解法2 - 构建大顶堆求 Top k问题

我们也可以通过构造一个大顶堆来解决,大顶堆上的任意节点值都必须大于等于其左右子节点值,即堆顶是最大值. 

所以我们可以重数组中取出 k 个元素构造一个大顶堆,然后将其余元素与大顶堆对比,如果小于堆顶则替换堆顶,然后堆化,所有元素遍历完成后,堆中的元素即为前 k 个最小值


```js
function findMinStock(stock, cnt) {
  const heap = []; // 定义一个空的堆

  // 将库存余量加入堆中
  for (let i = 0; i < stock.length; i++) {
    // 如果堆的大小小于 cnt,则直接将当前库存余量入堆
    if (heap.length < cnt) {
      heap.push(stock[i]);
      // 如果堆的大小已经达到 cnt,则构建大顶堆
      if (heap.length === cnt) {
        buildMaxHeap(heap);
      }
    } else {
      // 如果当前库存余量比堆中的最大值小,则将其替换为堆中的最大值,并重新构建大顶堆
      if (stock[i] < heap[0]) {
        heap[0] = stock[i];
        heapify(heap, cnt, 0);
      }
    }
  }

  return heap; // 返回堆中的最小 cnt 个库存余量
}
/**
 * 构建最大堆
 */
function buildMaxHeap(arr) {
  const len = arr.length;
  // 从最后一个非叶节点开始进行堆化操作, 往前操作
  for (let i = Math.floor(len / 2) - 1; i >= 0; i--) {
    heapify(arr, len, i);
  }

  return arr;
}

// 堆化函数
/*
* i 堆化的对象
*/
function heapify(arr, len, i) {
  let largest = i; // 初始化当前节点为最大值
  const left = 2 * i + 1; // 左子节点索引
  const right = 2 * i + 2; // 右子节点索引

  // 如果左子节点存在且大于当前节点,则更新最大值索引
  if (left < len && arr[left] > arr[largest]) {
    largest = left;
  }

  // 如果右子节点存在且大于当前节点或左子节点,则更新最大值索引
  if (right < len && arr[right] > arr[largest]) {
    largest = right;
  }

  // 如果最大值不是当前节点,则交换两个节点的值,并递归地对交换后的子树进行堆化操作
  if (largest !== i) {
    // swap(arr, i, largest);
    [arr[i], arr[largest]] = [arr[largest], arr[i]]
    heapify(arr, len, largest);
  }
}

```

- 时间复杂度: 遍历数组需要 O(n) 的时间复杂度,一次堆化需要 O(logk) 时间复杂度,所以利用堆求 Top k 问题的时间复杂度为 `O(nlogk)`
- 空间复杂度: `O(k)`

## 利用堆求 Top k 问题的优势

这种求 `Top k` 问题是可以使用排序来处理,但当我们需要在一个`动态数组`中求` Top k `元素怎么办喃?

动态数组可能会插入或删除元素,难道我们每次求 `Top k` 问题的时候都需要对数组进行重新排序吗?那每次的时间复杂度都为` O(nlogn)`

这里就可以使用堆,我们可以维护一个 K 大小的小顶堆,当有数据被添加到数组中时,就将它与堆顶元素比较,如果比堆顶元素大,则将这个元素替换掉堆顶元素,然后再堆化成一个小顶堆; 如果比堆顶元素小,则不做处理. 这样,`每次求 Top k 问题的时间复杂度仅为 O(logk)`

## 解法3 - 基于快速排序的 partition ！！！
```js
//  快排的 关键 在于先在数组中选择一个数字,接下来把数组中的数字分为两部分,比选择的数字小的数字移到数组的左边,比选择的数字大的数字移到数组的右边. [比第 k 个数字小的所有数组都位于数组的左边,比第 k 个数字大的都位于右边]
// 定义 partition 函数,用于将数组按照枢轴分为左右两部分,并返回枢轴的索引位置

/**
 * arr 表示待分区的数组,start 表示起始索引,end 表示结束索引
 * 实现了快速排序算法中的分区操作,用于将数组按照枢轴元素的大小进行分割
 */
function partition(arr, start, end) {
    let pivot = arr[start]; // 将第一个元素作为枢轴, 小于枢轴元素的部分和大于等于枢轴元素的部分
    //  left 表示小于枢轴元素的元素的最右边界,初始值为 start + 1; right 表示大于等于枢轴元素的元素的最左边界,初始值为 end
    let left = start + 1; // 初始化 i,表示小于枢轴的元素的最右边界
    let right = end
    while (1) {
        while (left <= end && arr[left] <= pivot) ++left;
        while (right >= start + 1 && arr[right] >= pivot) --right;
        //  如果 left 指针的位置大于等于 right 指针的位置,说明已经完成了一轮分区操作,此时退出循环. 
        if (left >= right) {
            break;
        }
        //  交换 left 和 right 指针所指向的元素,将小于枢轴元素的元素放到左边,大于等于枢轴元素的元素放到右边. 然后,将 left 指针向右移动一位,将 right 指针向左移动一位
        [arr[left], arr[right]] = [arr[right], arr[left]];
        ++left;
        --right;
    }
    //  将枢轴元素和 right 指针所指向的元素进行交换,使得枢轴元素放在正确的位置上
    [arr[right], arr[start]] = [arr[start], arr[right]];
    //  函数返回 right,即枢轴元素最终所在的索引
    return right;
}

/**
 * @param {number[]} arr 输入的数组
 * @param {number} k 获取最小的元素个数
 * @return {number[]} 返回最小的 k 个元素
 */
const getLeastNumbers = function(arr, k) {
    const length = arr.length;
    if (k >= length) return arr; // 如果 k 大于等于数组长度,直接返回原数组
    let left = 0, // 左边界
        right = length - 1; // 右边界
    let index = partition(arr, left, right); // 对整个数组进行分区操作,获取枢轴的索引位置, 完成之后,arr 已经根据枢轴值 进行分区
    while (index !== k) {
        if (index < k) {
            // 如果枢轴的索引位置小于 k
            left = index + 1; // 更新左边界; 因为是以left 作为基准值; 
            index = partition(arr, left, right); // 对左侧部分进行分区操作
        } else if (index > k) {
            // 如果枢轴的索引位置大于 k
            right = index - 1; // 更新右边界
            index = partition(arr, left, right); // 对右侧部分进行分区操作
        }
    }

    return arr.slice(0, k); // 返回最小的 k 个元素
};
O(n)
O(n)
```