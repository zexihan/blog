---
layout:     post
title:      "<>Leetcode - Reverse Linked List"
subtitle:   "Leetcode Note"
date:       2016-09-15
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Leetcode
    - Easy
---



## Reverse Linked List

Reverse a singly linked list.

Hint: A linked list can be reversed either iteratively or recursively. Could you implement both?

> Java solution: Iteration

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode reverseList(ListNode head) {
        if(head==null || head.next==null) return head;  
                 
        ListNode pre = head;  
        ListNode p = head.next;  
        pre.next = null;  
        ListNode nxt;  
        while(p!=null) {  
            nxt = p.next;  
            p.next = pre;  
            pre = p;  
            p = nxt;  
        }  
        return pre;  
    }  
}

```
> Java solution: Recursion