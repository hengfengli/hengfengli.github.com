---
layout: post
title: "SCIP Section 1.3"
category: [sicp]
tags: [sicp]
---
{% include JB/setup %}

<div><strong>1.3 Formulating Abstraction with Higher-Order Procedures</strong>
用高阶过程来构建抽象，Procedure在这里，应该也可以翻译成函数。</div>
<div><strong>1.3.1 Procedures as Arguments</strong></div>
<div>

这一节主要是讲，如何把procedure作为参数，将一些具有共同特征的函数，抽象成更高级的函数。（有点乱。。。）

下面是一个简单的计算三次方的函数。

</div>
<pre>(define (cube x) (* x x x))</pre>
<div>１.计算从a到b的整数和。</div>
<pre>(define (sum-integers a b)
  (if (&gt; a b)
      0
      (+ a (sum-integers (+ a 1) b))))</pre>
<div>２.计算从a到b的整数的三次方和。</div>
<pre>(define (sum-cubes a b)
  (if (&gt; a b)
      0
      (+ (cube a) (sum-cubes (+ a 1) b))))</pre>
<div>３.计算这样一个序列</div>
<div>$$\frac{1}{1\times 3}+\frac{1}{5\times 7} + \frac{1}{9\times 11} + ...$$</div>
<div>最终趋于$$\pi /8$$</div>
<pre>(define (pi-sum a b)
  (if (&gt; a b)
      0
      (+ (/ 1.0 (* a (+ a 2))) (pi-sum (+ a 4) b))))</pre>
<div>可以看到，这三个函数，其实蕴藏着一样的模式，</div>
<pre>(define (&lt;name&gt; a b)
  (if (&gt; a b)
      0
      (+ (&lt;term&gt; a)
         (&lt;name&gt; (&lt;next&gt; a) b))))</pre>
<div>其实，也就是累加的抽象。(The abstraction of summation of a series - "sigma notation")</div>
<div>$$\sum_{n=a}^{b}f(n)=f(a)+... +f(b)$$</div>
<div>写成scheme的话，就如下：</div>
<pre>(define (sum term a next b)
  (if (&gt; a b)
      0
      (+ (term a)
         (sum term (next a) next b))))</pre>
<div>这样就可以把函数当做参数传入。</div>
<pre>(define (inc n) (+ n 1))
(define (sum-cubes a b)
  (sum cube a inc b))

(define (identity x) x)
(define (sum-integers a b)
  (sum identity a inc b))

(define (pi-sum a b)
  (define (pi-term x)
    (/ 1.0 (* x (+ x 2))))
  (define (pi-next x)
    (+ x 4))
  (sum pi-term a pi-next b))</pre>
<div>可以从上面看到，以前的三个函数可以改成上面这种简洁的形式。有了这个sum的函数，我们可以把它应用到更多具有相同模式的地方。</div>
<div>例如，$$\int_{a}^{b}f=\left [ f(a+\frac{dx}{2}) + f(a+dx+\frac{dx}{2}) +f(a+2dx+\frac{dx}{2}) + ...  \right ]dx$$</div>
<div>可以写成：</div>
<pre>(define (integral f a b dx)
  (define (add-dx x) (+ x dx))
  (* (sum f (+ a (/ dx 2.0)) add-dx b)
     dx))</pre>
<div>这是函数式语言的一个重要的地方，让语言变得更加灵活，更具有表达力。</div>
<div>sssss</div>