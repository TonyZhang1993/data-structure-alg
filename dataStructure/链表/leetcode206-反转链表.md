## 反转链表
输入一个链表，反转链表后，输出新链表的表头. 

```js
//  迭代法 prev -> curr -> curr.next
//  解题思路:  将单链表中的每个节点的后继指针指向它的前驱节点即可
let reverseList = (head) => {
  //  存储当前节点之前的节点
  let prev = null
  let curr = head

  while (curr) {
    //  存储curr 后继节点
    let next = curr.next
    //  反转curr 的后继节点
    curr.next = prev
    
    //  变更prev, curr, 往后平移
    prev = curr
    curr = next
  }
  //  最后curr指向null, prev 指向最后一个节点
  head = prev

  return head
}

O(n)
O(1)

//  递归法
var reverseList = function(head) {
    if(!head || !head.next) return head

    var next = head.next
    // 递归反转
    var reverseHead = reverseList(next)
    // 变更指针
    next.next = head
    head.next = null
    return reverseHead
};

O(n)
O(1)
```
