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




