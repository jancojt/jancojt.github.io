---
layout: post
title: Dynamic Programming
date: 2015-02-17 10:54:00
comments: true
permalink: dynamic-programming
categories: posts
---

Getting acquainted the basic concepts of computer science is challenging, I'd like to take the time to better understand core concepts and perhaps help others reach a better understanding as well. For me, dynamic programming represents the next major leap in thought following iteration and recursion as a means of solving and thinking about complex problems. We'll take a look at the Fibonacci Series followed by common problems and applications of the concept. 

# Fibonacci Series

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script type="text/javascript"
  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
<p>
  $$F_{n} = F_{n-1} + F_{n-2}$$
  $$ 0, 1, 1, 2, 3, 5, 8, 13, 21, ... $$ 
</p>

To find the $nth$ Fibonacci number, or $F_{n}$, we have a recursive solution emerge:

```python 
def nth_fibonacci(n):
    if n <= 2: 
        return 1

    return nth_fibonacci(n-1) + nth_fibonacci(n-2)
```

For $F(6)$ we should return the value $8$ which exists at index $6$. The function will call itself until it is resolved to the base case of $n<=2$. 


