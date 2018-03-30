---
layout: post
title: "[HL-18] Guess number - DP again"
categories:
    - algorithm
tags:
    - dynamic programming
published: true
---

I met a difficult problem again relate to dynamic programming (DP), the [Guess
Number problem](https://leetcode.com/problems/guess-number-higher-or-lower-ii/).  

Let's first have a look the problem.

~~~

We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number I picked is higher
or lower.

However, when you guess a particular number x, and you guess wrong, you pay $x.
You win the game when you guess the number I picked.

Example:

n = 10, I pick 8.

First round:  You guess 5, I tell you that it's higher. You pay $5.
Second round: You guess 7, I tell you that it's higher. You pay $7.
Third round:  You guess 9, I tell you that it's lower. You pay $9.

Game over. 8 is the number I picked.

You end up paying $5 + $7 + $9 = $21.
Given a particular n ≥ 1, find out how much money you need to have to guarantee
a win.

~~~

I got confused about "find out how much money you need to have to guarantee a win."
A hint says "minimize the maximum loss you could possibly face".
After reading the hints, I still couldn't get a clue.
However, there are some great explanation on the discussion forum [A](https://discuss.leetcode.com/topic/52760/still-confused-about-guarantee-a-win-why-minimize-the-max-cost/3)
and
[B](https://discuss.leetcode.com/topic/53369/how-does-one-think-up-dp-solutions-for-these-types-of-problems/3).
These answers really help me understand the problem.

It is important to __"minimize the maximum loss you could possibly face"__.
If you choose a strategy, this strategy will need different
amounts of money to deal with all possible guess numbers. The money for a
guaranteed win is calculated for the worst case.
A clear example is given in
[this discussion](https://discuss.leetcode.com/topic/52760/still-confused-about-guarantee-a-win-why-minimize-the-max-cost/3).

~~~

Assume n = 10, a strategy with a guarantee win can be described as a decision
tree.

                7
              /   \
            3       9
           / \     /  \
          1   5   8   10
          |   /\
          2  4  6

In the above example, what is the maximum loss for all guess numbers?

1: 7 + 3 = 10
2: 7 + 3 + 1 = 11
3: 7 = 7
4: 7 + 3 + 5 = 15
5: 7 + 3 = 10
6: 7 + 3 + 5 = 15
7: 0
8: 7 + 9 = 16 (worst case)
9: 7
10: 7 + 9 = 16 (worst case)

Thus, the maximum loss is $16.

An important observation:

maxloss(7) = 7 + max(maxloss([1..6]), maxloss([8..10]))

maxloss(3) = 3 + max(maxloss([1..2]), maxloss([4..6]))

maxloss(9) = 9 + max(maxloss([8]), maxloss([9]))

and so forth ...

This is an important sub-problem structure for DP problems.

~~~

Our task is to __find the minimum of maximum loss among all
possible strategies__.

Assume the guess number is in 1..n. We choose k as the first split number
where k can be any number between 1 and n. So we can get the following
__sub-problem relationship__:

<span style="display: block; text-align: center;">minimax(1,n) = min([k + max(minimax(1,k-1), minimax(k+1,n)) for k in 1..n])</span>

I convert it into code:

<script src="https://gist.github.com/HengfengLi/24850cd7ade0285e4f57f41afd58b93d.js"></script>

However, this recursive solution is time-consuming. With the __memorization__ technique,
I come up the following solution:

<script src="https://gist.github.com/HengfengLi/bb74e2d9782a6235a172b4a1afa28310.js"></script>

This solution is accepted by the online judge system. But function calls require a lot
amount of space. So I convert the code to a __DP__ solution without using recursion:

<script src="https://gist.github.com/HengfengLi/4c4b1f283a9d30d482e73860f7617284.js"></script>

In summary, recursion -> memorization -> DP is a common strategy to incrementally
improve your solution to solve a DP problem. The key of a DP problem is to
find out the __sub-problem structure or relationship__.
