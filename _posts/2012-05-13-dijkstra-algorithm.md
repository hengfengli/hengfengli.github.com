---
layout: post
title: "Dijkstra Algorithm"
categories: [algorithm]
tags: [dijkstra]
---

由于traffic simulator的项目里，涉及到一个求最短路径的问题。
所以，我实现了一下比较著名的Dijkstra Algorithm。
它是一个single source shortest paths问题，也就是说，它求的是一个出发点的所有最短路径。
它是一个典型的贪婪算法(greedy algorithm)，它将问题分解成很多个步骤，每个步骤里都选择
最优解，最终所有的最优解形成最短路径。但我们知道，有时候局部的最优，并不一定会带来整体的最优。

其实在之前做machine translation的作业时候，也遇到了这样一个问题。
在n-gram language model里面，给一个单词的set，重组这个单词set，让它变成一个句子。
我采用的就是一种greedy algorithm的算法，就拿2-gram来说。句子的开头会有一个NULL单词，代表着句子的开头。首先，先找出NULL后面最有可能出现的单词。
然后，再以刚才找的单词为context，再找最可能出现的单词，直到所有的单词都用完了。
这样，虽然每次找到的都是最有可能出现的单词，但就整个句子来讲，这个句子不一定是最有可能的。

实现了Weiss的Data Structure and Algorithm Analysis (JAVA)上的算法，并且自己修改了一下基础结构，分别用Adjacent Matrix和Adjacent List，加上Priority Queue实现了。

[DEMO地址](https://github.com/HengfengLi/Dijkstra-Algorithm)