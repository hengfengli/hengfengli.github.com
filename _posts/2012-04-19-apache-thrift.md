---
layout: post
title: "Apache Thrift - 跨语言服务开发框架"
categories: [computer] 
tags: [thrift]
---

在写project时候，遇到一个很有趣的问题，用php写的web层如何与java写的逻辑层通信呢？

php写的都是静态的网页格式，必须放在apache服务器上用zend引擎解析。而这边java写的逻辑层，并不只是一个简单的程序，而是一个需要持续运行的服务器。

其实我需要找的是一个通信的中间件，我在搜索解决方案时，找到了很多解决方法，如soap, RMI, Quercus, Php/Java Bridge, Avro, Thrift, Protocol Buffer. 

最后，我看上了Thrift, 它是由facebook开发，现在已经贡献给Apache了。它主要是支持跨语言服务开发的框架，结合了software stack(是一种结构，就像tcp/ip协议的stack结构一样。)和代码自动生成的引擎，使得各个语言有效和无缝地工作。它现在支持的语言有C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk, Ocami, Delphi等等。

下面说一下我的安装及使用过程：

### 下载Thrift的源文件

去[官网](http://thrift.apache.org/)下Thrift

### 安装Thrift依赖的软件

官网上教的如何安装Boost，我没有弄成功。主要是要找到bjam这个东西，我没找到。但是，我找到用macport安装其所需的方法。

{% highlight bash %}
sudo port install boost
sudo port install libevent
sudo port install pkgconfig
{% endhighlight %}

这样就能把所需的东西装好了。

### 配置和编译Thrift

{% highlight bash %}
./configure --prefix=/usr/local/ --with-boost=/usr/local --with-libevent=/usr/local --disable-static
make
make install
{% endhighlight %}

### 运行tutorial

如果要运行tutorial的话，最好把README都仔细的看一遍，我就是吃了没看的亏，走了点弯路。

{% highlight bash %}
thrift -r -gen php tutorial.thrift
{% endhighlight %}

其实这个就是一个接口文件，由thrift自己的语言定义接口的数据类型，然后由代码生成的引擎自动翻译成相关语言。

但是，我发现还是有一个小小的问题，当我生成php代码的时候，如果在thrift中为php定义了namespace的话，那么所有php自动生成的类，前面都会加一个tutorial_前缀，所以导致运行程序的时候报错，说找不到相关的类。但在java中就不会有这种问题。

{% highlight bash %}
namespace php simulator
{% endhighlight %}

还有一个问题就是，实际上php的服务器端，我并没有跑起来，因为运行php服务端的时候，貌似没有正确监听端口，导致client总是报connection refused，无法连接9090端口。

有趣的是，我能正确地运行java的服务器端，这样才使得我能继续下去，试成功的时候挺开心的。

{% highlight c %}
/**
 * The first thing to know about are types. The available types in Thrift are:
 *
 *  bool        Boolean, one byte
 *  byte        Signed byte
 *  i16         Signed 16-bit integer
 *  i32         Signed 32-bit integer
 *  i64         Signed 64-bit integer
 *  double      64-bit floating point value
 *  string      String
 *  binary      Blob (byte array)
 *  map<t1,t2>  Map from one type to another
 *  list<t1>    Ordered list of one type
 *  set<t1>     Set of unique elements of one type
 *
 * Did you also notice that Thrift supports C style comments?
 */

namespace java simulator

service Simulator {

   void setBound(1:double left, 2:double top, 3:double right, 4:double bottom),

   list<i32> getCars()

}
{% endhighlight %}

我现在定义的接口比较简单，因为现阶段所需交换的数据比较少。但是，可以看到，
它提供的数据类型还是十分强大的。而且更有趣的是，我发现在tutorial里面的shared.shrift，它实现了一个类似dictionary的东西。基本上thrift定义接口的语言，还是遵循c的风格，比较好理解的。

### Reference

其实我基本上是看着官网的指导和README，完成的实例运行。我还是搜索了一些资料，如下：

[Apache Thrift - 可伸缩的跨语言服务开发框架](http://www.chineselinuxuniversity.net/articles/49624.shtml)

[Apache Thrift 学习](http://smallmin.iteye.com/blog/1121085)

[Installing Cassandra and Thrift on Snow Leopard - A Quick Start Guide](http://jetfar.com/installing-cassandra-and-thrift-on-snow-leopard-a-quick-start-guide/)

[Apache Thrift Tutorial - A PHP Client](http://chanian.com/2010/05/13/thrift-tutorial-a-php-client/)

[Apache Thrift入门1-架构&介绍](http://www.javabloger.com/article/apache-thrift-architecture.html)

[Apache Thrift入门2-Java代码实现例子](http://www.javabloger.com/article/thrift-java-code-example.html)