---
layout: post
title: "0 - Minimal set cover problem"
categories: [programming]
tags: [algorithm]
published: true
---

我在之前的interview中遇见了这道题，可以说是我的第一道phone interview题目。
当时并没有反应过来，是minimal set cover问题。最近一个印度同事在准备interview，
所以问了我一下之前的经验，我把遇到过的题目发给她了。结果她一看，就能立马反应出，
这是一道minimal set cover问题，find a set covering that uses the fewest sets，
这是个NP-hard问题。突然，感慨自己的经验不够，做算法题太少，面对题目的反应和分析能力不够，
还得多加训练。所以，也促成我想开始写一点关于自己遇到的算法问题，以及如何解决的方法。
毕竟，教是最好的学嘛。

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

我这里给一个简单实现，更多的可以看看wikipedia。更困难的题目是每个set都有一个weight，
求既要覆盖U的所有元素，并且要求最小weight，可以看看[3]。

---

__Reference__

[0] [Set cover problem on Wikipedia](https://en.wikipedia.org/wiki/Set_cover_problem)

[1] [Polynomials on MATH is FUN](http://www.mathsisfun.com/algebra/polynomials.html)

[2] [什么是P问题、NP问题和NPC问题](http://www.matrix67.com/blog/archives/105)

[3] [Set Cover Problem Set 1 (Greedy Approximate Algorithm)](http://www.geeksforgeeks.org/set-cover-problem-set-1-greedy-approximate-algorithm/)
