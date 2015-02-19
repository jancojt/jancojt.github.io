---
layout: post
title: LeetCode OJ Problem 1 - Two Sum
date: 2015-02-18 10:00:00
comments: true
categories: posts
tags:
- leetcode
---
Ongoing series to provide comprehensive answers to LeetCode Online Judge problems in Python.
I hope this helps you improve; attempting to explain these solutions definitely helps me.

*Note\: Naming conventions follow LeetCode's submission specifications (classes/camelCase)*

Problem 1: [Two Sum](https://oj.leetcode.com/problems/two-sum/) 

<pre class=code>
Given an array of integers, find two numbers such that they add up to a specific target number.
The function twoSum should return indices of the two numbers such that they add up to the target,
where index1 must be less than index2.
Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.
<b>Input:</b> numbers={2, 7, 11, 15}, target=9
<b>Output:</b> index1=1, index2=2
</pre>


### Naive: $\mathcal{O}(n\^2)$ 

The naive approach has 2 pointers ($i$, $j$), we keep one in place and move the other through the remaining values checking for our sum. We return a tuple of the indices correctly summing to our target. 

```python
class Solution:
    def twoSum(self, a, tar):
        i = 0 
        j = 0 
        while i < len(a) - 1:
            j = i + 1 
            while j < len(a):
                if a[i] + a[j] == tar:
                    return (i+1, j+1) # problem specifies indices at 1
                j += 1 
            i += 1 
        return (-1, -1) 
```

### Sort and Binary Search: $\mathcal{O}(n\log n)$ 

We could also sort the data in *nlogn* time and perform searches on the array in *logn* time using binary search. For every element we would search for the difference between it and the target sum. This solution will lead to timeouts in the OJ. Left this code out because sorting the array necessitates tracking with a hash map. 

### Shove Into Dictionary: $\mathcal{O}(n)$ 

We keep track of each element as we traverse the array, storing its index in a hash map. At each iterative step, we check our hash map for the difference between the target sum and the current value, e.g. if a[3] = 5, target sum = 8, we would like to see a stored value of 3.

```python
class Solution:
    def twoSum(self, a, tar):
        hashed_comps = {}
        for i, num in enumerate(a):
            if tar - num in hashed_comps:
                return sorted((i + 1, hashed_comps[tar - num] + 1))
            else:
                hashed_comps[num] = i
        return (-1, -1)
```
