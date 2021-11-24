---
layout: post
title: "[Algorithm] Leetcode Add Two Numbers"
subtitle: "Add Two Numbers"
categories: study
tags: algo
---
> Leetcode Two Sum

[https://leetcode.com/problems/add-two-numbers/](https://leetcode.com/problems/add-two-numbers/)  


## 1. 나의 풀이

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        
        int sum = l1.val + l2.val;
        
        ListNode rtn = new ListNode();
        rtn.val = sum % 10;
        int carry = sum / 10;
        
        add(rtn, l1.next, l2.next, carry);
        
        return rtn;
    }
    
    
    private void add(ListNode node, ListNode l1, ListNode l2, int carry) {
        
        if(l1 == null && l2 == null){
            if (carry > 0) {
                node.next = new ListNode(carry);
            }
            return;
        }
        
        int sum = carry;
        
        if (l1 != null) {
            sum += l1.val;
        }
        
        if (l2 != null) {
            sum += l2.val;
        }
        
        node.next = new ListNode(sum % 10);
        carry = sum / 10;
        
        if (l1 != null && l2 != null){
            add(node.next, l1.next, l2.next, carry);
        } else if (l1 == null && l2 != null) {
            add(node.next, null, l2.next, carry);
        } else if (l1 != null && l2 == null) {
            add(node.next, l1.next, null, carry);
        }         
    }
}
```

## 2. Solution
```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
}
```


