---
layout: post
title: "学习Haskell"
categories: [programming]
tags: [haskell]
published: false
---

不得不说，虽然刚开始学Haskell的时候，觉得好难，经常会想，为什么会有这样的写法，为什么要这样？
但经过一段时间的学习后，开始觉得Haskell很有意思了，而且觉得不那么难了。

对于c和java来说，我们总是在想应该如何去做这件事情，每一个步骤是什么，一系列的步骤成就了我们的解决方案。
而对于Haskell，则需要完全换种思维方式，要对问题进行更细微抽象，要知道如何去描述问题。

例如，下面这个程序，找最大值，

{% highlight haskell %}
maximum :: (Ord a) => [a] -> a
maximum []  = error "maximum of empty list"
maximum [x] = x
maximum (x:xs) = max x (maximum xs)
{% endhighlight %}


不管其他的细节，它的概念就是，一个List的最大值，等于“第一个元素”与“其sublist的最大值”相比较。这就是不同的编程思维方式，递归（recursion）的理念。当然不光这些，Haskell里面还有很多好用的东西，List Comprehension, filter, map, foldr等等，包括Monads（我还没学，只不过听人家说，是需要革新理念的东西，就像第一次学C的指针一样。）。

再看一个快速排序，

{% highlight haskell %}
quicksort :: (Ord a) => [a] -> [a]
quicksort [] = []
quicksort (x:xs) =
  let smallerSorted = quicksort [a | a <- xs, a <= x]
      biggerSorted  = quicksort [a | a <- xs, a >  x]
  in  smallerSorted ++ [x] ++ biggerSorted
{% endhighlight %}

这个是我第一次对Haskell表现力折服的一个程序。。。如果用C和java写，这代码得多难理解，多少行啊。。。

Haskell真的不错，我想我会一直坚持学下去的。记得经常有同学问我，学这个东西对找工作有帮助吗？有什么用吗？我不知道，我只是觉得编程是件好玩的事情，像我们这种国内本科毕业过来的，只能接触到类C语言的概念。能尝试一下新的东西，不是很好吗？干嘛那么功利呢，只要学的过程很开心就好了。记得看乔布斯的一些故事的时候，他不也没想到，他学的设计字体的课程会对他以后设计Mac有帮助吧。人生就是很多点，说不定哪一天就把你以前积累的那些点，连成一条线了。

因为用了pygments来高亮代码，所以先用Haskell来测试一下。

