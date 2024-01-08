
https://leetcode.cn/leetbook/read/linked-list/jbex5/

## 环形链表

给你一个链表的头节点 head ，判断链表中是否有环。
如果链表中存在环 ，则返回 true 。 否则，返回 false 。

> 最简单的一种方式就是快慢指针，慢指针针每次走一步，快指针每次走两步，如果相遇就说明有环，如果有一个为空说明没有环。


## 思路
- 快慢指针
- 标记法 - 遍历，标记flag属性

```js

/**
 * @param {ListNode} head
 * @return {boolean}
 */
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    if (!head || !head.next) {
        return false;
    }
    let slow = head;
    let fast = head.next;

    while (slow !== fast) {
        if (!fast || !fast.next) {
            return false
        }
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
};

//  删除节点的方法判断是否有环
var hasCycle = function(head) {
  if (!head || !head.next) {
    return false
  }

  if (head.next === head) return true

  let nextNode = head.next
  head.next = head

  return hasCycle(nextNode)
}



//  如果慢指针每次走m步，快指针每次走n步，只要m≠n，好像都能通过，如果在这样写下去，那么答案就无穷多了
```
删除法 见证环形链表；
![Alt text](/images/删除法见证环形链表.png)
