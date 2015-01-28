---
layout: post
title: "Scaling Instagram - 易扩展的Instagram"
categories: [computer]
tags: [instagram]
published: false
---

当所有人都在惊讶Instagram被Facebook收购的时候，我十分好奇，究竟是什么原因造成这个公司的效率如此之高，13个人的公司服务着几千万的用户。

2个人，没有任何后端的经验，从python开始，构建了第一个项目CrimeDesk SF。仅仅只用了一个一般的服务器，第一天就有25k的注册量。

### 初期错误

毫无疑问，刚开始的时候肯定也有很多低级的错误。在slides中提到了以下几点：

1.   忘记favicon.icon，所以导致每次访问都会报404错误。

2.   `ulimit -n size`，设置内核可以同时打开的文件描述符的最大值，例如size为4092。

3.   `memcached -t 4`，Number of threads to use to process incoming requests.

4.   `prefork/postfork`，查了一下，fork-on-request ("postfork") and child-pool ("prefork")，在我看来，postfork就是来请求了，才fork一个thread去处理。而prefork则是，预先就fork好一定量的thread，然后，当有request来的时候，pool中的thread去处理请求，其实就相当于thread pool的概念。

### 技术选型

他们将后端移到了EC2，也介绍了Instagram技术的选型，这个非常重要，因为当用户数量上去的时候，你的应用很有可能crash掉：

{% highlight bash %}
Nginx & 
Redis & 
Postgres & 
Django
{% endhighlight %}


之后是

{% highlight bash %}
Nginx & HAProxy &
Redis & Memcached &
Postgres & Gearman &
Django
{% endhighlight %}

### Instagram的哲学

1.   Simplicity

2.   Optimize for minimal operational 

3.   Instrument everything

### 数据库相关

早期主要是依赖于django ORM和postgresql。

当图片数量不断上涨的时候，需要做vertical partitioning, django db routers make it pretty easy. 

{% highlight python %}
def db_for_read(self, model):
  if app_label == 'photos':
  	return 'photodb'
{% endhighlight %}

当photodb超出60GB的时候，他们做了horizontal partitioning，也就是Sharding（中文是“分片”的意思）。

那么，他们遇到了什么问题呢？

1.   Data retrieval - hard to know what your primary access patterns will be w/out any usage

2.   What happens if one of your shards gets too big?

他们采用了pre-split的方法，上千个逻辑上的shards, map到几个物理的shards上。

PG一个非常好的特征：schemas

{% highlight python %}
- database:
  - schema:
    - table:
      - columns
{% endhighlight %}

__Lesson:__ Take tech/tools you know and try first to adapt them into a simple solution. 

### 最后一些建议

1.   extensive unit-tests and functional tests

2.   keep it DRY

3.   loose coupling using notifications / signals

4.   do most of our work in Python, drop to C when necessary

5.   frequent code reviews, pull requests to keep things in the 'shared brain'

6.   extensive monitoring 

5个前端工程师，2个后端工程师，3000W的用户数，多于10亿被Facebook收购。我想其实Simplicity才是他们的成功秘诀，包括从slides上，我也学到了一些东西，以前总喜欢在幻灯片上放很多东西，其实到最后，坐在下面的观众都看不清幻灯片上写的什么。而Instagram的幻灯片上，没有太多花俏的东西，每张幻灯片基本上就一句话。

简洁，简单才是取胜之道。

### Reference

[Mike Krieger - Scaling -Instagram](http://www.scribd.com/doc/89025069/Mike-Krieger-Instagram-at-the-Airbnb-tech-talk-on-Scaling-Instagram)