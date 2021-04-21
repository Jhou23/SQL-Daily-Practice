## Problem Introduction
Given the head of a singly linked list, reverse the list, and return the reversed list.

## Explanation
```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]

Input: head = [1,2]
Output: [2,1]

Input: head = []
Output: []
```

## Code
Iteration
```
def reverseList(self, head: ListNode) -> ListNode:
        prev = None
        curr = head
        while curr:
            temp = curr.next  # record nextnode of curr one
            curr.next = prev  # redirect nextnode to prev one
            prev = curr  # move prev node to curr node
            curr = temp  # move curr node to next node
        return prev
```
Time: O(n)
Space: O(1)

Recursion
```
def reverseList(self, head: ListNode) -> ListNode:
    if not head or not head.next:
    return head

    cur = self.reverseList(head.next)
    head.next.next = head
    head.next = None
    return cur
```
Time: O(n)
Space: O(n)

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
