[题目](https://leetcode.cn/problems/sort-list/description/)

给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 . 

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
var sortList = function(head) {
    let tmp = []
    let cur = head

    while(cur !== null) {
        tmp.push(cur.val)
        cur = cur.next
    }

    tmp.sort((a, b) => a - b)
    cur = head
    tmp.forEach((i) => {
        cur.val = i
        cur = cur.next
    })

    return head


};

O(nlogn)
O(n)
```