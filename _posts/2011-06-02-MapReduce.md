---
layout: post
title: "MapReduce编程模型"
categories: [problem]
tags: [mapreduce]
---
{% include JB/setup %}

### 1. 什么是MapReduce？

MapReduce是google的员工研究出来的一种适用于分布式计算的编程模型，可以有效地处理大规模的数据集。这个模型由Map和Reduce函数组成，Map函数接收一对关键字/值的参数（key/value pairs），产生一个临时中间的关键字/值参数的集合（a set of intermediate key/value pairs，这里不知道怎么翻译，其实就相当于一个hash集合，关键字与值一一对应）。而Reduce函数接收一个intermediate key，以及这个intermediate key所对应的全部intermediate values。其实可以想象成这样，假设有很多Map函数，他们都对于一个intermediate key产生了许多value，而Reduce函数就是将这些values整合到一起，这样intermediate key就只与一个value一一对应。我刚开始看论文的时候，也是一头雾水，最关键的就是不知道intermediate key/values到底指的是什么。不过，看了论文中的一个例子，我就开窍了。让我们来看看吧。

![](https://lh5.googleusercontent.com/-gyTRi6-m-yI/Teda2o3fgjI/AAAAAAAAAI4/yUtmlaWcYao/s640/Untitled.jpg)

例如，对于一个大规模的文档集合，如何统计词频？用MapReduce模型的伪代码如下：

<div class="highlight"><pre>
map(String key, String value):
   // key: document name
   // value: document contents
   for each word w in value:
       EmitIntermediate(w, “1”);

reduce(String key, Iterator values):
    // key: a word
    // values: a list of counts
    int result = 0;
    for each v in values:
        result += ParseInt(v);
    Emit(AsString(result));
</pre></div>

可以看到map函数传入的参数是key: 文档名，value:文档内容

输出的是，key:单词，value:在这个文档中的词频（“１”是因为假设这个单词在这个文档中出现一次）。

那么，reduce函数做什么工作呢？将所有map函数产生的key和value，简化成一个key和value，也就是key:单词，value:在这个文档集合中的词频。

光从输入输出可以看成如下：

<div class="highlight"><pre>
map       (k1, v1)          -&gt;list(k2,v2)
reduce    (k2,list(v2))     -&gt;list(v2)
</pre></div>

这样可以看到，通过对于大规模文档集的分割，可以利用cluster并行高效地处理，（之前一直在想cluster，grid和cloud到底有啥区别，简单点说，其实cluster是一个若干台计算机通过局域网组成的集合，而grid和cloud是通过Internet。cluster是centralized，grid和cloud是decentralized。而cloud应该是grid的一种形式，因为grid应该只是对于企业内部而言，cloud则是对外提供商业服务）。

### 2. MapReduce的案例

#### Distributed Grep:

分布式的模式匹配，相当于在大规模文档中，给定模式（pattern），找出所有匹配的行。

map 输入：文档名，文档内容　输出：一个文档匹配的行

reduce　输入：分离的结果集　 输出：一个结果集

#### Count of URL Access Frequency:

统计URL进入的频率，也就是说，这个URL的活跃程度。

map　 输入：网页请求的日志　  输出：URL，这个log中出现的频率

reduce　输入：URL，分离的结果集　输出：URL，total count

#### Reverse Web-Link Graph:

反向web链接图，对于一个URL来说，就是在一个大规模网页集合中，找出所有指向这个URL的链接的集合，也就是说，。但是，并不是找出一个URL就可以了，而是需要找出所有这样的pairs。

map　 输入：网页　  输出：&lt;URL, 这个网页中指向这个URL的链接&gt;

reduce　输入：URL，分离的结果集　输出：&lt;URL,网页集合中所有指向链接&gt;

#### Term-Vector per Host:

这个term呢，其实就是我们在用搜索引擎的时候，搜索引擎将你的query分割成几个term，然后去查找term出现频率最高的网站，综合比较给出结果排名，（真正的算法十分复杂，不会像我说的这么简单）。
而term vector则是一个集合，这个集合包含了，在一个文档或者一个文档集合中，最重要的terms及它们的频率。

map　 输入：document　 输出：hostname, term vector

reduce　输入：hostname, list(term vector) 　输出：hostname，term vector

reduce函数会将所有一个host的term vector加起来，丢掉那些频率低的terms，最终形成一个&lt;hostname, term vector&gt;，也就是网站的term vector。

#### Inverted Index:

这个是一个web search里的结构，是用来建立word与文档之间的索引。

map　 输入：document　 输出： &lt;word, document ID&gt;

reduce  输入： &lt;word, document ID&gt; pairs 输出：&lt;word, list(document ID)&gt;

这些工作其实就是google每天在做的事情。这上面的很多概念，都是这个学期我的一门很难的课里讲的，这门课叫knowledge technology，前半个学期将string matching, approximate matching, web search，后半个学期讲data mining的东西，如classifier，cluster，非常难，但非常有意思。


### Reference:

Jeffrey Dean and Sanjay Ghemawat,《MapReduce: Simplified Data Processing on Large Clusters》