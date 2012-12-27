---
layout: post
title: "SICP Exercise 1.1-1.8"
category: sicp
tags: [sicp]
published: false
---
{% include JB/setup %}

<strong>Exercise 1.1</strong>

按照书上的代码，输出结果如下：
<div id="_mcePaste">10</div>
<div id="_mcePaste">12</div>
<div id="_mcePaste">8</div>
<div id="_mcePaste">3</div>
<div id="_mcePaste">6</div>
<div id="_mcePaste">19</div>
<div id="_mcePaste">#f</div>
<div id="_mcePaste">4</div>
<div id="_mcePaste">16</div>
<div id="_mcePaste">6</div>
<div id="_mcePaste">16</div>
<div>其中#f代表false。</div>
<div><strong>Exercise 1.2</strong></div>
<pre>(/ (+ 5
      4
      (- 2
         (- 3
            (+ 6
               (/ 4 5)))))
   (* 3
      (- 6 2)
      (- 2 7)))</pre>
<div>输出结果为-37/150。</div>
<div><strong>Exercise 1.3</strong></div>

    (define (square x) (* x x))

    (define (maxTwo a b c)
      (cond ((and (&lt; a b) (&lt; a c))
             (+ (square b) (square c)))
            ((and (&lt; b c) (&lt; b a))
             (+ (square a) (square c)))
            (else (+ (square a) (square b)))))

<div><strong>Exercise 1.4</strong></div>
<div>a+|b|</div>
<div><strong>Exercise 1.5</strong></div>
<pre>(define (p) (p))

(define (test x y)
  (if (= x 0)
      0
      y))

(test 0 (p))</pre>
<div>这题给出了这么一个程序，主要考察的就是，applicative-order和normal-order计算的区别。如果是applicative-order，那么将会先计算(p)，那么就会进入一个死循环，there is no an end loop as applicative-order。在normal-order下的话，就会先把procedure展开，直到遇到primitive operators时，才进行计算，那么在遇到if的时候，程序进行判断，predicates为0，所以程序求值得0，(p)也就没有调用。</div>
<div><strong>Exercise 1.6</strong></div>
<div>题目的意思是，为什么不能用cond来代替if呢？</div>
<pre>(define (new-if predicate then-clause else-clause)
  (cond (predicate then-clause)
        (else else-clause)))</pre>
<div>这是，题目中给出，用cond替代if的语句，它同样实现了if的功能。那么如果运行下面这个程序，将会发生什么事情呢？</div>
<pre>(define (sqrt-iter guess x)
  (new-if (good-enough? guess x)
          guess
          (sqrt-iter (improve guess x)
                     x)))</pre>
<div>关键的区别，就在于if是作为一种special form存在的，它不会先去计算所有子表达式的值，而如果你定义一个procedure来替代的话，procedure就会计算所有子表达式的值。</div>
<div>在书的page 9，讲到了如何计算combinations，</div>
<blockquote>
<div>To evaluate a combination, do the following:</div>
<div>1. Evaluate the subexpressions of the combination.</div>
<div>2. Apply the procedure that is the value of the leftmost subexpression (the operator) to the arguments that are the values of the other subexpressions (the operands).</div></blockquote>
<div>也就是说，先计算组合的子表达式的值，然后再将操作符运用到这些值上。但是，这里存在一些特殊的形式(special forms)，它们不遵守上面的计算规则。例如，我们在计算(define x 3)的时候，并不是把define运用到它的两个参数x和3上面，因为define的作用就是把x和3联系起来，所以，(define x 3) 不是一个combinations。</div>
<div>回到题目中来，在进行new-if之前，解释器会计算它的三个子表达式，其中第三个参数sqrt-iter就会被调用，那么程序将陷入一个死循环，因为新调用的sqrt-iter，又会在进行它的new-if判断前，再调用sqrt-iter。</div>
<div>这道题的关键还是在于，applicative-order and normal-order evaluation和special forms，这两个概念。</div>
<div><strong>Exercise 1.7</strong></div>
<div>对于前面求平方根sqrt中，good-enough?过程对于特别小或者特别大的数字，没有效果。在最初的good-enough?中，是求guess的两次方与x的差，来估算精度的。但是，对于特别大的数字，它的差可能根本无法精确到good-enough?要求的精度0.001。对于特别小的数字，在guess平方后，它可能满足了精度，但实际上，guess离x平方根还差得很远。例如，当求0.0006时，得到的结果是0.037，而不是预期的结果0.024。</div>
<div>题目中，提到另一种方法，An alternative strategy for implementing good-enough? is to watch how guess changes from one iteration to the next and to stop when the change is a very small fraction of the guess.</div>
<pre>(new-guess - old-guess) / old-guess &lt; 0.001</pre>
<div>也就是说，根据变化的比例来确定精度。下面，是修改的程序，</div>
<pre>(define (sqrt x)
  (define (square x) (* x x))
  (define (average x y)
    (/ (+ x y) 2))
  (define (good-enough? improve-guess guess)
    (&lt; (abs (/ (- improve-guess guess) guess)) 0.001))
  (define (improve guess)
    (average guess (/ x guess)))
  (define (sqrt-iter guess)
    (if (good-enough? (improve guess) guess)
        guess
        (sqrt-iter (improve guess))))
  (sqrt-iter 1.0))</pre>
<div>通过修改的程序，计算0.0006就可以得到预期的结果0.024。</div>
<div><strong>Exercise 1.8</strong></div>
<div>这个题目是，按照计算sqrt的方法，根据所给公式计算cube，修改了一下上面的程序，就好了。</div>
<pre>(define (cube x)
  (define (square x) (* x x))
  (define (good-enough? improve-guess guess)
    (&lt; (abs (/ (- improve-guess guess) guess)) 0.001))
  (define (improve y)
    (/ (+ (/ x (square y))
          (* 2 y))
       3))
  (define (cube-iter guess)
    (if (good-enough? (improve guess) guess)
        guess
        (cube-iter (improve guess))))
  (cube-iter 1.0))</pre>