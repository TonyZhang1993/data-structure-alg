
## 相交链表

[题目](https://leetcode.cn/leetbook/read/linked-list/jjbj2/)
[题目](https://leetcode.cn/problems/intersection-of-two-linked-lists/description/)

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。

-  set 方法
-  两个链表同步遍历，最后都指向对方的head， 如果有交点的话就会相遇
-  统计两个链表长度差值，长链表先行，再遍历判断是否相等

```js
// set 方法
var getIntersectionNode = function(pHead1, pHead2) {
  let visited = new Set()

  while(pHead1) {
      visited.add(pHead1)
      pHead1 = pHead1.next
  }

  while(pHead2) {
      if (visited.has(pHead2)) {
          return pHead2
      }
      pHead2 = pHead2.next
  }

  return null
};

//  两个链表同步遍历，最后都指向对方的head， 如果有交点的话就会相遇
var getIntersectionNode = function(pHead1, pHead2) {
  if (!pHead1 || !pHead2) return null

  let p1 = pHead1
  let p2 = pHead2

  while(p1 !== p2) {
    p1 = p1 ? p1.next : pHead2
    p2 = p2 ? p2.next : pHead1
  }

  return p1
}

//  null === null; undefined === undefined
```