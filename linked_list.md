### Merge Two Sorted Lists

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

This question is super hard for me because I am not familiar with linked list. I did not work it out, so I just copied the answer online:

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode()
        pointer = dummy
        while list1 and list2:
            if list1.val <= list2.val:
                pointer.next = list1
                list1 = list1.next
            else:
                pointer.next = list2
                list2 = list2.next
            pointer = pointer.next
        if list1:
            pointer.next = list1
        else:
            pointer.next = list2
        return dummy.next
```

There are several things I can summarize:

1. `None` can be used in condition statement directly!
2. There is a difference between `&`  and `and`!

| Aspect               | `&`                                            | `and`               |
| -------------------- | ---------------------------------------------- | ------------------- |
| **Type**             | Bitwise operator                               | Logical operator    |
| **Operands**         | Works with integers, Booleans, or NumPy arrays | Works with any type |
| **Short-circuiting** | No                                             | Yes                 |
| **Result**           | A computed value (bitwise or element-wise)     | One of the operands |
