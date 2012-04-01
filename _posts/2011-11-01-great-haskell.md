---
layout: post
title: "Great Haskell"
<<<<<<< HEAD
category: [programming]
=======
category: programming
>>>>>>> 3d2abd91cb23246e0718a8c06199a17986a4505b
tags: [haskell]
---
{% include JB/setup %}

不得不说，虽然刚开始学Haskell的时候，觉得好难，经常会想，为什么会有这样的写法，为什么要这样？
但经过一段时间的学习后，开始觉得Haskell很有意思了，而且觉得不那么难了。

对于c和java来说，我们总是在想应该如何去做这件事情，每一个步骤是什么，一系列的步骤成就了我们的解决方案。
而对于Haskell，则需要完全换种思维方式，要对问题进行更细微抽象，要知道如何去描述问题。

例如，下面这个程序，找最大值，

<div class="highlight"><pre><span class="nf">maximum</span> <span class="ow">::</span> <span class="p">(</span><span class="kt">Ord</span> <span class="n">a</span><span class="p">)</span> <span class="ow">=&gt;</span> <span class="p">[</span><span class="n">a</span><span class="p">]</span> <span class="ow">-&gt;</span> <span class="n">a</span>
<span class="nf">maximum</span> <span class="kt">[]</span>  <span class="ow">=</span> <span class="ne">error</span> <span class="s">&quot;maximum of empty list&quot;</span>
<span class="nf">maximum</span> <span class="p">[</span><span class="n">x</span><span class="p">]</span> <span class="ow">=</span> <span class="n">x</span>
<span class="nf">maximum</span> <span class="p">(</span><span class="n">x</span><span class="kt">:</span><span class="n">xs</span><span class="p">)</span> <span class="ow">=</span> <span class="n">max</span> <span class="n">x</span> <span class="p">(</span><span class="n">maximum</span> <span class="n">xs</span><span class="p">)</span>
</pre></div>

不管其他的细节，它的概念就是，一个List的最大值，等于“第一个元素”与“其sublist的最大值”相比较。这就是不同的编程思维方式，递归（recursion）的理念。当然不光这些，Haskell里面还有很多好用的东西，List Comprehension, filter, map, foldr等等，包括Monads（我还没学，只不过听人家说，是需要革新理念的东西，就像第一次学C的指针一样。）。

再看一个快速排序，

<div class="highlight"><pre><span class="nf">quicksort</span> <span class="ow">::</span> <span class="p">(</span><span class="kt">Ord</span> <span class="n">a</span><span class="p">)</span> <span class="ow">=&gt;</span> <span class="p">[</span><span class="n">a</span><span class="p">]</span> <span class="ow">-&gt;</span> <span class="p">[</span><span class="n">a</span><span class="p">]</span>
<span class="nf">quicksort</span> <span class="kt">[]</span> <span class="ow">=</span> <span class="kt">[]</span>
<span class="nf">quicksort</span> <span class="p">(</span><span class="n">x</span><span class="kt">:</span><span class="n">xs</span><span class="p">)</span> <span class="ow">=</span>
  <span class="kr">let</span> <span class="n">smallerSorted</span> <span class="ow">=</span> <span class="n">quicksort</span> <span class="p">[</span><span class="n">a</span> <span class="o">|</span> <span class="n">a</span> <span class="ow">&lt;-</span> <span class="n">xs</span><span class="p">,</span> <span class="n">a</span> <span class="o">&lt;=</span> <span class="n">x</span><span class="p">]</span>
      <span class="n">biggerSorted</span>  <span class="ow">=</span> <span class="n">quicksort</span> <span class="p">[</span><span class="n">a</span> <span class="o">|</span> <span class="n">a</span> <span class="ow">&lt;-</span> <span class="n">xs</span><span class="p">,</span> <span class="n">a</span> <span class="o">&gt;</span>  <span class="n">x</span><span class="p">]</span>
  <span class="kr">in</span>  <span class="n">smallerSorted</span> <span class="o">++</span> <span class="p">[</span><span class="n">x</span><span class="p">]</span> <span class="o">++</span> <span class="n">biggerSorted</span>
</pre></div>

这个是我第一次对Haskell表现力折服的一个程序。。。如果用C和java写，这代码得多难理解，多少行啊。。。

Haskell真的不错，我想我会一直坚持学下去的。记得经常有同学问我，学这个东西对找工作有帮助吗？有什么用吗？我不知道，我只是觉得编程是件好玩的事情，像我们这种国内本科毕业过来的，只能接触到类C语言的概念。能尝试一下新的东西，不是很好吗？干嘛那么功利呢，只要学的过程很开心就好了。记得看乔布斯的一些故事的时候，他不也没想到，他学的设计字体的课程会对他以后设计Mac有帮助吧。人生就是很多点，说不定哪一天就把你以前积累的那些点，连成一条线了。

因为用了pygments来高亮代码，所以先用Haskell来测试一下。

