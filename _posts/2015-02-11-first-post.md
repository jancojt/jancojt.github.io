---
layout: post
title: Jekyll and GitHub
date: 2015-02-11 15:00:00
categories: posts
---

Decided to start up a blog to keep me accountable with my various endeavors. Hosted with github pages using the Jekyll blogging framework. Got some really nice formatting for code snippets and LaTeX from a few libraries and tweaked them to my liking.

### Test Code
```python
#!/usr/bin/python3 

import random
def print_rand():
    print(random.randint(1, 99))
if __name__ == '__main__':
    print_rand()
```

### Test Terminal

<pre class=terminal>
> echo "testing terminal prompt"
</pre>

### Test LaTeX 
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script type="text/javascript"
  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
<p>
When $a \ne 0$, there are two solutions to \(ax^2 + bx + c = 0\) and they are
$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$
$$
\left[
\begin{matrix}
1 & x & x^2 \\
1 & y & y^2 \\
1 & z & z^2 \\
\end{matrix}
\right]
$$
</p>

Pretty sweet.
