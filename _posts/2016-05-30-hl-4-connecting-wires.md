---
layout: post
title: "[HL-4] Connecting wires"
categories: 
    - algorithm
tags: 
    - queue
    - stack
published: true
---

There is an interesting problem for designing a greedy algorithm. 
You can find the problem description from 
[this link](http://grid.cs.gsu.edu/~ywang/Greedy%20Algorithms.pdf). 

* There are <font color="red">n</font> white dots and <font color="red">n</font> black dots, equally spaced, in a line
* You want to connect each white dot with some one black dot, with a minimum total length of “wire”
* Example: 

![](/assets/img/hl-4-connecting-wires.png)

* How can we design a greedy algorithm for doing this? 
* Does the algorithm guarantee an optimal solution? 

## A Proof for Using Queue

You can find the answer from a [Quora's link](https://www.quora.com/How-would-you-design-a-greedy-algorithm-which-connects-each-black-dot-with-a-white-dot-so-that-the-total-length-of-wires-used-to-form-such-connected-pairs-is-minimal). 
I just rephrase its answer here. 

It proves that we should **_match ith white dot to the ith black dot_**. 

Assume that we have 2 white dots, $w_0$ and $w_j$, 2 black dots, $b_0$
and $b_i$.  $w_0$ and $b_0$ are the leftmost dots of white color and
black color, respectively. $w_0$ is leftmost one for all dots. A
solution is to connect $w_0$ with $b_i$  and $w_j$ with $b_0$. 

To prove the optimal solution, we only need to prove that we should
match  $w_0$ to $b_0$. Then, it is also true for the remaining dots,
such as $w_1$ and $b_1$. 

There are three possible positions for $w_j$: 

* If $w_j < b_0, b_i$, 

![](/assets/img/hl-4-case-1.png)

$$TL_1 = L(w_0, b_i) + L(w_j, b_0) = b_i - w_0 + | w_j - b_0 | = b_i - w_0 + b_0 - w_j$$

$$TL_2 = L(w_0, b_0) + L(w_j, b_i) = b_0 - w_0 + | w_j - b_i | = b_0 - w_0 + b_i - w_j$$

$$TL_1 - TL_2 = 0$$

* If $b_0 < w_j < b_i$, 

![](/assets/img/hl-4-case-2.png)

$$TL_1 = b_i - w_0 + w_j - b_0$$

$$TL_2 = b_0 - w_0 + b_i - w_j$$

$$TL_1 - TL_2 = 2 (w_j - b_0) > 0 $$

* If $b_0, b_i < w_j$, 

![](/assets/img/hl-4-case-3.png)

$$TL_1 = b_i - w_0 + w_j - b_0$$

$$TL_2 = b_0 - w_0 + w_j - b_i$$

$$TL_1 - TL_2 = 2 (b_i - b_0) > 0 $$

Therefore, if we match $w_0$ to $b_0$, this brings us the least length. 
We can use queue to implement this method. 

However, as we can see from the above examples, another data structure, 
stack, can also give us the optimal solution. What would be a good 
proof for using a stack? 

[This link](http://www.tau.ac.il/~csedu/greedy_trap.pdf) (Section 3) provides
a good explanation for transforming to a parenthesis-matching problem. 
This can be done using a stack. 

[This link](http://www.cs.uni.edu/~wallingf/teaching/cs3530/sessions/session25.html)
is another place that discussed this problem. 




