[题目](https://leetcode.cn/problems/middle-of-the-linked-list/description/)

## 876. 链表的中间结点

- 快慢指针
- 一次遍历得到长度，下一次遍历找到中间值

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var middleNode = function(head) {
    if (!head) return head

    let n = 0
    let cur = head
    while(cur) {
        cur = cur.next
        n++
    }

    let index = Math.floor(n / 2)
    cur = head
    while(index) {
        cur = cur.next
        index--
    }

    return cur
};

var middleNode = function(head) {
    if (!head) return head

    let slow = head, fast = head
    //  fast 比 slow 快1倍
    while(fast && fast.next) {
        slow = slow.next
        fast = fast.next.next
    }

    return slow
};

O(n)
O(1)
```