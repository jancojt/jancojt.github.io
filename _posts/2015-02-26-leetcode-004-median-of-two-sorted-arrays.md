---
layout: post
title: LeetCode OJ Problem 4 - Median of Two Sorted Arrays
date: 2015-02-26 18:43:00
comments: true
categories: posts
permalink: leetcode-4-median-of-two-sorted-arrays
tags:
- leetcode
---
Ongoing series to provide comprehensive answers to LeetCode Online Judge problems in Python.
I hope this helps you improve; attempting to explain these solutions definitely helps me. 
Solutions compatible with the OJ will be in Python 2.7, those that are not are in Python 3.4.

*Note\: Naming conventions follow LeetCode's submission specifications (classes/camelCase)*

Problem 4: [Median of Two Sorted Arrays](https://oj.leetcode.com/problems/median-of-two-sorted-arrays/) 

<pre class=code>
There are two sorted arrays A and B of size m and n respectively. 
Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).
<b>Input:</b> 2 sorted arrays
<b>Output:</b> float
</pre>

### Sort of Naive: $\mathcal{O}(m + n)$ 

The naive implementation resolves to linear time. As we are given arrays we know the length of each in constant time and therefore know where the median should resolve. We loop through the index of our median (med_idx), at each stage we maintain our previous value m1. m2 is updated in our logic block. If we've traversed array a completely, we know we must continue traversing b, and vice versa. If we have not traversed either completely we simply check for the lower value and increment pointer i (corresponding to array a) or j (corresponding to array b).

Depending on an even or odd total length we return the average of 2 values or a singular value. 
$$ a = {0, 1, 2}\quad b = {3, 4, 5}\quad   median = (2+3)/2 = 2.5$$
$$ a = {0, 1, 2}\quad b = {3, 4}\quad  median = 2.0 $$

Not bad, but not good enough for a solution in the requested log(m+n) time. The OJ judge will accept this solution, if you tweak the return to include return (m + n) / 2.0 to fit python 2.x standard for returning floats from division. Python 3.x defaults to float values for division. 
 
```python
def naive_median(a, b):
    i = 0
    j = 0 

    med_idx = (len(a) + len(b)) // 2 

    m1 = -1
    m2 = -1 

    for k in range(med_idx+1):
        m1 = m2
        if i == len(a) :
            m2 = b[j]
            j += 1 
        elif j == len(b):
            m2 = a[i]
            i += 1
        else:
            if a[i] < b[j]:
                m2 = a[i]
                i += 1
            else:
                m2 = b[j]
                j += 1

    if (len(a) + len(b)) % 2 == 0:
        return (m1 + m2) / 2
    else:
        return float(m2)
```
### Divide and Conquer: $\mathcal{O}(log(m+n))$ 

References:
[Code](http://yucoding.blogspot.com/2013/01/leetcode-question-50-median-of-two.html)
[Technique](https://www.youtube.com/watch?v=_H50Ir-Tves)

We can use a technique to find the median of our sorted arrays by using the fact that each is sorted and that we know our median in constant time. We figure out our median index as we did in our semi-naive solution.

$$k = (m + n) / 2$$

The variables m and n represent the lengths of the input arrays for arrays a and b respectively. We then have two pointers pa and pb for each array. We can then begin eliminating a portion of either array through a simple comparison. 

Let's say we have arrays: 
a = [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]
b = [ 5, 6, 7, 8, 9, 10 ]

The recursive calls for arrays a and b resolve as follows:

1)
k = 9 
a = [ 5, 6, 7, 8, 9, 10 ] 
b = [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]
pa = 4, pb = 5

2)
k = 4
a = [ 5, 6, 7, 8, 9, 10 ] 
b =  [ 5, 6, 7, 8, 9, 10] 
pa = 2, pb = 2

3)
k = 2 
a = [7, 8, 9, 10] 
b = [5, 6, 7, 8, 9, 10]
pa = 1, pb = 1

4)
k = 1
a = [7, 8, 9, 10] 
b = [6, 7, 8, 9, 10]
median = 6

So, in step 1, at we see that the index of k = 9 puts us 1 step above the median of our merged array, which is [ 0, 1, 2, 3, 4, 5, 6, 6, **7**, 7, 8, 8, 9, 9, 10, 10 ]. Our pa is k halved, which is 4, we set pb to be k minus that value, or 5. Checking the indicies below each pointer gives us our comparison values. At indices 3 and 4, in a and b respectively, we have values of 8 and 4. Since 8 > 4, we know that the elements in b underneath pb can be removed. We return a and b[pb:] as the next arrays in the recursion and subtract the values under pointer pb as k - pb. 


```python
class Solution:
    def findMedianSortedArrays(self, a, b):
        m = len(a) 
        n = len(b) 

        if (m + n) % 2 == 0:
            return (self.fmsa(a, b, (m+n)/2) +
                    self.fmsa(a, b, (m+n)/2 + 1)) / 2.0 
        else:
            return self.fmsa(a, b, (m+n)/2 + 1)

    def fmsa(self, a, b, k): 
        if len(a) > len(b):
            return self.fmsa(b, a, k)

        if len(a) == 0:
            return b[k-1]
        if k == 1:
            return min(a[0], b[0])

        pa = min(k/2, len(a))
        pb = k - pa

        if a[pa-1] <= b[pb-1]:
            return self.fmsa(a[pa:], b, k-pa)
        else:
            return self.fmsa(a, b[pb:], k-pb)
```
