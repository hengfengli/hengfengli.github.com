---
layout: post
title: "[HL-17] Word ladder and clone graph - BFS"
categories:
    - algorithm
tags:
    - BFS
published: true
---

The new feature, **mock interview**, from Leetcode is really cool! It forces me to
solve the problem within the time limitation.

I met a BFS problem "127. Word Ladder". I was trying to use DFS to generate
all possible transformation paths. I got "Time Limit Exceeded". Then, I looked
at [this link](https://discuss.leetcode.com/topic/42623/compact-python-solution/2),
and realized that this should use a BFS method (using a queue).

The code is as follows:

<script src="https://gist.github.com/HengfengLi/76e8e492473618697eda0bcfa58b608a.js"></script>

There is another question "133 Clone Graph" about BFS.

I wrote an incorrect BFS to solve this question. Again, understanding BFS
doesn't mean that you can code it correctly (I forgot that we have to trace
visited nodes while traversing). This answer is referenced from [this link](https://discuss.leetcode.com/topic/23945/python-solutions-bfs-dfs-iteratively-dfs-recursively/2).

<script src="https://gist.github.com/HengfengLi/c666e18b0e3e0d954b62d6702516c033.js"></script>
