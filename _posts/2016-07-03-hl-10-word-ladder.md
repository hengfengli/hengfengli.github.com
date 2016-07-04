---
layout: post
title: "[HL-10] Word ladder and clone graph - BFS"
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

~~~
Given two words (beginWord and endWord), and a dictionary's word list, 
find the length of shortest transformation sequence from beginWord 
to endWord, such that:

Only one letter can be changed at a time
Each intermediate word must exist in the word list
For example,

Given:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]
As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.

Note:
Return 0 if there is no such transformation sequence.
All words have the same length.
All words contain only lowercase alphabetic characters.

Analysis: 

Start from a word: 
    1) check if the current word is the target; if it is, then return the length
    2) check next possible words in the dictionary (only one character difference)
    3) remove next words from the wordlist, and push next words into the queue

E.g., 

                hit
                 |       
                hot
             /       \
            dot      lot
            |         |
            dog      log
              \     /
                cog
~~~

The code is as follows: 

~~~python
import collections

class Solution(object):
    def ladderLength(self, beginWord, endWord, wordList):
        """
        Take home lesson:
            BFS: queue
            DFS: stack or recursive functions
        """
        wordList.add(endWord)
        queue = collections.deque([[beginWord, 1]])
        while queue:
            word, length = queue.popleft()
            if word == endWord:
                return length
            for i in range(len(word)):
                for c in 'abcdefghijklmnopqrstuvwxyz':
                    # This is a key improvement. 
                    # Originally, I check for every item in the word list
                    # to see if its distance to 'word' equals to 1. 
                    next_word = word[:i] + c + word[i+1:]
                    if next_word in wordList:
                        wordList.remove(next_word) # prevent going backward
                        queue.append([next_word, length + 1])
        return 0
~~~

There is another question "133 Clone Graph" about BFS. 

~~~
Clone an undirected graph. Each node in the graph contains a label and a 
list of its neighbors.


OJ's undirected graph serialization:
Nodes are labeled uniquely.

We use # as a separator for each node, and , as a separator for node label
and each neighbor of the node.
As an example, consider the serialized graph {0,1,2#1,2#2,2}.

The graph has a total of three nodes, and therefore contains three parts 
as separated by #.

First node is labeled as 0. Connect node 0 to both nodes 1 and 2.
Second node is labeled as 1. Connect node 1 to node 2.
Third node is labeled as 2. Connect node 2 to node 2 (itself), thus forming 
a self-cycle.
Visually, the graph looks like the following:

       1
      / \
     /   \
    0 --- 2
         / \
         \_/
~~~

I wrote an incorrect BFS to solve this question. Again, understanding BFS
doesn't mean that you can code it correctly (I forgot that we have to trace
visited nodes while traversing). This answer is referenced from [this link](https://discuss.leetcode.com/topic/23945/python-solutions-bfs-dfs-iteratively-dfs-recursively/2). 

~~~python
import collections

# Definition for a undirected graph node
class UndirectedGraphNode(object):
    def __init__(self, x):
        self.label = x
        self.neighbors = []

class Solution(object):
    
    def cloneGraph(self, node):
        """
        :type node: UndirectedGraphNode
        :rtype: UndirectedGraphNode
        
        Correct BFS! 
        
        Take home lesson: BFS needs to trace visited nodes!!!
        """
        if not node:
            return node
        
        start = UndirectedGraphNode(node.label)
        dic = {node:start} # a dictionary for recording visited nodes
        queue = collections.deque([node])
        
        while queue:
            node = queue.popleft()
            
            for neighbor in node.neighbors:
                
                if neighbor in dic: # visited before, not need to add into queue
                    dic[node].neighbors.append(dic[neighbor])
                else:
                    dic[neighbor] = UndirectedGraphNode(neighbor.label)
                    dic[node].neighbors.append(dic[neighbor])
                    queue.append(neighbor)
                    
        return start
~~~

