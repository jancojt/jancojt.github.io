---
layout: post
title: LeetCode OJ Problem 2 - Add Two Numbers
date: 2015-02-18 15:00:00
comments: true
categories: posts
permalink: leetcode-2-add-two-numbers
tags:
- leetcode
---
Ongoing series to provide comprehensive answers to LeetCode Online Judge problems in Python.
I hope this helps you improve; attempting to explain these solutions definitely helps me.

*Note\: Naming conventions follow LeetCode's submission specifications (classes/camelCase)*

Problem 2: [Add Two Numbers](https://oj.leetcode.com/problems/add-two-numbers/) 

<pre class=code>
You are given two linked lists representing two non-negative numbers. 
The digits are stored in reverse order and each of their nodes contain a single digit. 
Add the two numbers and return it as a linked list.

<b>Input:</b> (2 -> 4 -> 3) + (5 -> 6 -> 4)
<b>Output:</b> 7 -> 0 -> 8 
</pre>

### Traverse and Remember the Carry: $\mathcal{O}(m+n)$ 

While our node is not None (or Null), we traverse the lists simultaneously and sum each node's value. Since the result must also be a linked list with each node representing a base 10 digit, we must maintain a carry variable. The carry will never exceed 1 since our max value of 9 + 9 + 1 gives us 19. We store the result in a linked list that grows as we iterate. 

So, for the example input, 2 + 5 = 7, we add this to a result list, 6 + 4 = 10, we add 0 to the result list and save our carry, 4 + 3 = 7, plus the carry gives us 8, we store 8 as our final value. 

If the lists were the same size, we would not need our clean up loops. Because list sizes vary, our iteration can terminate before we have traversed both fully. For that reason, we have clean up loops to handle varying array sizes and at the end we append our carry if it is greater than 0.  

```python 
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # @return a ListNode
    def addTwoNumbers(self, h1, h2):
        h3 = ListNode(0)
        res = h3
        carry = 0 

        while h1 and h2:
            summation = carry + h1.val + h2.val
            carry = summation // 10  

            res.next = ListNode(summation % 10)

            h1 = h1.next 
            h2 = h2.next
            res = res.next

        while h1:
            summation = carry + h1.val
            carry = summation // 10

            res.next = ListNode(summation % 10)
            
            h1 = h1.next
            res = res.next

        while h2: 
            summation = carry + h2.val
            carry = summation // 10

            res.next = ListNode(summation % 10)

            h2 = h2.next
            res = res.next

        if carry:
            res.next = ListNode(carry)

        return h3.next
```
