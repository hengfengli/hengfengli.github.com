---
layout: post
title: "[HL-10] Word ladder - BFS"
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