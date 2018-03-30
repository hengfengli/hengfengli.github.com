---
layout: post
title: "[HL-9] Minimal set cover problem"
categories:
    - algorithm
tags:
    - set
published: true
---

This is a minimal set cover problem. I met such a problem during one
of my coding interviews.

---

__Problem:__ Given a group of sets S, find a minimal set covering that
includes all elements in U.

__Input:__

S = {s1, s2, s3, s4}

s1 = {1,2,3}

s2 = {2,4}

s3 = {3,4}

s4 = {4,5}

U = {1,2,3,4,5}

__Output:__

ans = {s1, s4}

__Solution:__ Greedy Algorithm (polynomial time approximation)

Idea: at each stage, choose the set that contains the largest
number of uncovered elements.

~~~ python
"""
Minimal set cover problem.
"""

# At each stage, find a set that includes largest number of uncovered
# elements.
def set_cover(S, U):
    ans = []
    while len(U) != 0:
        # find a set that maximumly covers
        intersects = [U.intersection(x) for x in S]
        lengths = map(len, intersects)
        max_index = lengths.index(max(lengths))

        # remove it from the entire set and uncovered set
        ans.append(S.pop(max_index))
        U.difference_update(intersects[max_index])

    return ans

s1 = set([1,2,3])
s2 = set([2,4])
s3 = set([3,4])
s4 = set([4,5])
S = [s1,s2,s3,s4]
U = set([1,2,3,4,5])

print set_cover(S, U)
~~~

Here, I give a simple implementation. You can find more on Wikipedia.
A more difficult problem is that every set has a weight, so it requires
to cover all items in U with a minimal total weight (refer to [3]).

---

__Reference__

[0] [Set cover problem on Wikipedia](https://en.wikipedia.org/wiki/Set_cover_problem)

[1] [Polynomials on MATH is FUN](http://www.mathsisfun.com/algebra/polynomials.html)

[2] [What is P, NP, and NPC problems (Chinese)](http://www.matrix67.com/blog/archives/105),
great explanation of those problems.

[3] [Set Cover Problem Set 1 (Greedy Approximate Algorithm)](http://www.geeksforgeeks.org/set-cover-problem-set-1-greedy-approximate-algorithm/)
