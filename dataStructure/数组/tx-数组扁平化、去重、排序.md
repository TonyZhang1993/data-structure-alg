## 题目 

已知如下数组：var arr = [ [1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14] ] ] ], 10];
- review
编写一个程序将数组扁平化去并除其中重复部分数据，最终得到一个升序且不重复的数组

## 思路分析

方法一：
```js
// 扁平化；flat() 方法对node版本有要求，至少需要12.0以上
const flattenArr = (arr) => arr.flat(Infinity)

// 去重 Array.from(new Set(arr))
const unique = (arr) => [...new Set(arr)]

// 排序
const sort = (arr) => arr.sort((a, b) => a-b)

//  函数组合!!!!
const compose = (...fns) => {
  return (initValue) => {
    return fns.reduceRight((y, fn) => fn(y), initValue)
  }
}

const flatten_unique_sort = compose(sort, unique, flattenArr)

// 测试
var arr = [ [1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14] ] ] ], 10]
console.log(flatten_unique_sort(arr))


//  tip:
array1.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
  initialValue,
);

accumulator
上一次调用 callbackFn 的结果。在第一次调用时，如果指定了 initialValue 则为指定的值，否则为 array[0] 的值。

currentValue
当前元素的值。在第一次调用时，如果指定了 initialValue，则为 array[0] 的值，否则为 array[1]。



--------改写， 写进一个函数里
const flatten_unique_sort = (arr) => {
  const flattenArr = (arr) => arr.flat(Infinity)

  // 去重 Array.from(new Set(arr))
  const unique = (arr) => [...new Set(arr)]

  // 排序
  const sort = (arr) => arr.sort((a, b) => a-b)

  let rel = flattenArr(arr)
  rel = unique(rel)
  return sort(rel)
}

```

方法二
```js
function newArray(arr) {
  //  扁平化，array.some() + concat来实现这个flat()，这个对node版本的限制比较低，可行性较高
  while(arr.some(Array.isArray)) {
    arr = [].concat(...arr)
  }

  arr = [...new Set(arr)].sort((a, b) => a-b)

  return arr


}

//  tip
[].concat(1,2) === [].concat([1,2])  -> [1, 2]

```

```
tip:
1#
array.some() 方法测试数组中是否至少有一个元素通过了由提供的函数实现的测试。如果在数组中找到一个元素使得提供的函数返回 true，则返回 true；否则返回 false。它不会修改数组。


2#
reduceRight(callbackFn, initialValue)

callbackFn 参数

accumulator
上一次调用 callbackFn 的结果。在第一次调用时，如果指定了 initialValue 则为指定的值，否则为数组最后一个元素的值。

currentValue
数组中当前正在处理的元素。

index
正在处理的元素在数组中的索引。

array
调用了 reduceRight() 的数组本身。

3#
sort() 默认排序是将元素转换为字符串，然后按照它们的 UTF-16 码元值升序排序。
```