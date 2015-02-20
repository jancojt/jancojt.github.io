---
layout: post
title: LeetCode OJ Problem 3 - Longest Substring Without Repeating Characters 
date: 2015-02-20 13:47:00
comments: true
categories: posts
permalink: leetcode-3-longest-substring-without-repeating-characters
tags:
- leetcode
---
Ongoing series to provide comprehensive answers to LeetCode Online Judge problems in Python.
I hope this helps you improve; attempting to explain these solutions definitely helps me.

*Note\: Naming conventions follow LeetCode's submission specifications (classes/camelCase)*

Problem 3: [Longest Substring Without Repeating Characters](https://oj.leetcode.com/problems/longest-substring-without-repeating-characters/) 

<pre class=code>
Given a string, find the length of the longest substring without repeating characters.
For example, the longest substring without repeating letters for "abcabcbb" is "abc",
which the length is 3. For "bbbbb" the longest substring is "b", with the length of 1.
<b>Input:</b> string
<b>Output:</b> integer
</pre>

### Naive: $\mathcal{O}(n\^3)$ 

The naive implementation gives us a chance to look at some interesting sub problems. We want to generate every possible substring, then check those substrings for duplicate characters. The below example is not formatted for the Judge, but is more of an explanatory tool for what we are trying to optimize. 

The function "check\_sub" iterates through a string, saving letters in a dictionary, or hash map, if it hits a duplicate the function returns False, else continuing on returning True if the loop completes without a hitch. 

The function "max\_sstring" uses 2 loops to return all substrings and checks each with the helper function "check\_sub". The maximum non-duplicate substring length is checked and recorded as we progress, finally outputting the maximal value.

We see that we have 2 loops to find the substrings and 1 loop to check for duplicates giving us cubic time complexity.   

```python
#!/usr/bin/python3

def check_sub(s):
    dup_hash = {}
    for i in range(len(s)):
        if s[i] in dup_hash:
            return False
        else:
            dup_hash[s[i]] = 0 

    return True

def max_sstring(s):
    max_len = 0 
    for i in range(len(s)):
        for j in range(i+1, len(s) + 1): 
            if check_sub(s[i:j]):
                if j - i > max_len:
                    max_len = j - i 

    return max_len

if __name__ == '__main__':
    s = "abccc"
    print(max_sstring(s))
```

### The Moving Frame: $\mathcal{O}(n)$ 

In the following solution, we keep track of two pointers that point to the beginning and end of our longest contiguous substring. Think of this as a frame moving through our string. 

For example, take "abcdabcd". We can clearly see that our longest unique substring is "abcd" which has a maximum length of 4. Let's look at how the function will process this. We pass by our corner cases of None and single character, and initialize pointers i and j to 0 and 1. Our hash map contains "a" with it's value set to 0 (its index). We see that "b" is not present in the hash map and add it in with its index as its value. We increment our end of substring pointer j and continue traversing the string. Once we get to the second "a" we see that it was present in our hashmap at index 0. 

At this point we see a duplicate character, so we check our substring length and update max_len if we indeed have a larger substring than we started out with. We then set our pointer i to the duplicate character's index and keep j as is. Notice that we do not update our hash map nor do we increment j. We only want to increment i so that we bypass this duplicate value. Notice the "visited[s[j]] >= i", this check ensures that we only care about duplicate values in our current substring.

For the string "abcdabcd", our moving frame keeping track of the longest substring would look like this\:

ab, abc, abcd, abcda, bcda, bcdab, cdab, cdabc, dabc, dabcd, abcd

Also note that we are ok with off by 1 errors, our maximum substring with no duplicates is 4 in this case. Our complexity will be linear as we will never traverse the string more than 2n times for each pointer i and j. This simplifies asymptotically to O(n) complexity.

```python
class Solution:
    def lengthOfLongestSubstring(self, s):
        if len(s) < 1:
            return 0
        if len(s) == 1:
            return 1
        i = 0 
        j = 1 
        visited = {}
        visited[s[i]] = i 
        max_len = 1 
        while i < len(s) and j < len(s):
            if s[j] in visited and visited[s[j]] >= i:
                if j - i > max_len:
                    max_len = j - i 
                i = visited[s[j]] + 1
            else:
                visited[s[j]] = j 
                j += 1 

        if j - i > max_len:
            max_len = j - i 
        return max_len
```
