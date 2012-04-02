---
layout: post
title: "SICP Section 1.1"
category: [sicp]
tags: [sicp]
---
{% include JB/setup %}

1. Every powerful language has <strong>three mechanisms</strong> for accomplishing this:
<ul>
	<li><strong>primitive expressions</strong>, which represent the simplest entities the language is concerned with,</li>
	<li><strong>means of combination</strong>, by which compound elements are built from simpler ones, and</li>
	<li><strong>means of abstraction</strong>, by which compound elements can be named and manipulated as units.</li>
</ul>
这也就是说，每个强大的语言，必须实现三个基本机制：第一点，元语句表达式（最基本的数据及操作，包括整数，浮点数，加减乘除等等）。第二点，可以通过那些简单的元语句表达式，构建复杂表达式（通过括号可以将简单的运算，构建复杂运算表达式）。第三点，可以给这些复杂表达式命名，并且可以将它们作为单元来操作（其实就是指定义函数）。

<strong>2. Prefix Notation</strong>

Scheme的表达式，都是利用前缀表示法的，也叫波兰表示法。（在大一的时候，我学的《C语言程序设计》这本书中，就有后缀表示法，也叫逆波兰表示法。）

也就是说，

(+ 137 349)

<em>486</em>

(- 1000 334)

<em>666</em>

(* 5 99)

<em>495</em>

(/ 10 5)

<em>2</em>

(+ 2.7 10)

<em>12.7</em>

斜体表示的是输出结果。

(+ (* 3 (+ (* 2 4) (+ 3 5))) (+ (- 10 7) 6))

如果全部写在一行的话，十分难以辨认，所以，也可以写成如下：
<pre><code>(+ (* 3
        (+ (* 2 4)
            (+ 3 5)))
    (+ (- 10 7)
        6))
</code></pre>
上面这种写法，就是操作符在前，然后第一个操作数紧接着操作符，第二个操作数换行，对齐第一个操作数缩进。刚开始写Scheme，确实有点不习惯，太多括号了，有点眼花缭乱的感觉。

<strong>3. Define Procedures</strong>
<pre lang="scheme">(define pi 3.14159)
(define radius 10)
(* pi (* radius radius))
(define circumference (* 2 pi radius))
circumference</pre>
第3行和第5行输出的结果分别是，314.159和62.8318

    (define (square x) (* x x))

    (define (sum-of-squares x y)
    (+ (square x) (square y)))
    (sum-of-squares 3 4)

    (define (f a)
    (sum-of-squares (+ a 1) (* a 2)))

    (f 5)

第5行和第10行输出结果分别是，25和136。define后面，紧跟着的第一个括号内，就是函数的命名和参数，其中第一个标识符就是函数名，其后的都是参数。第二个括号内，就是函数的具体内容。

<strong>4. The Substitution Model for Procedure Application</strong>
<blockquote><span style="font-style: normal;">To apply a compound procedure to arguments, evaluate the body of the procedure with each formal parameter replaced by the corresponding argument.</span></blockquote>
也就是说，计算参数的值，并将参数的值代入到过程中去。例如，

(f 5)

f是我们之前描述过的过程，这个过程的body是，

(sum-of-squares (+ a 1) (* a 2))

那么，我们将用argument 5来替代过程中的参数(formal parameter)，就会变成如下，

(sum-of-squares (+ 5 1) (* 5 2))

接下里，通过计算，(+ 5 1) 算出6，(* 5 2))算出10，将这两个作为参数代入到过程sum-of-squares中去。

(+ (square 6) (square 10))

通过square的定义，可以归约(reduce)到，

(+ (* 6 6) (* 10 10))

最终得到，

(+ 36 100)

这只是一个简单的概念，帮助我们理解procedure application是如何工作的，并不能完全描述解释器(interpreter)是如何工作的。

5. Applicative Order versus Normal Order

这是secion 1.1的一个重点，讲如何在procedures调用的过程中，如何进行运算顺序。

Applicative order, 书上给出的解释是，the interpreter first evaluates the operator and operands and then applies the resulting procedure to the resulting arguments. 也就是之前所说的，先计算操作符和操作数，然后将结果作为参数传递。

而另外一种方法，normal order，书上给出的解释是，An alternative evaluation model would not evaluate the operands until their values were needed. Instead it would first substitute operand expressions for parameters until it obtained an expression involving only primitive operators, and would then perform the evaluation. 也就是说，直到遇到元操作符，才进行运算，否则，则将整个表达式，作为参数传递。

对两种方法简易的表达就是，

Applicative order =&gt; "evaluate the arguments and then apply"

Normal order =&gt; "fully expand and then reduce"

编译器实际使用的是applicative order。

还是之前那个例子，用normal order的运算顺序的话，如下

(f 5)

(sum-of-squares (+ 5 1) (* 5 2))

(+    (square (+ 5 1))      (square (* 5 2))   )

(+    (* (+ 5 1) (+ 5 1))    (* (* 5 2) (* 5 2))  )

然后，

(+           (* 6 6)               (* 10 10))

(+               36                     100)

136

通过上面的例子可以看到，在normal order下，(+ 5 1)和(* 5 2)需要计算两次，这样需要additional efficiency。

<strong>6. Conditional Expressions and Predicates</strong>
<pre lang="scheme">(define (abs x)
  (cond ((&gt; x 0) x)
        ((= x 0) 0)
        ((&lt; x 0) (- x))))

(define (abs x)
  (cond ((&lt; x 0) (- x))
        (else x)))

(define (abs x)
  (if (&lt; x 0)
      (- x)
      x))</pre>
(cond (&lt;p1&gt; &lt;e1&gt;)

(&lt;p2&gt; &lt;e2&gt;)

...

(&lt;pn&gt;&lt;en&gt;))

(if &lt;predicate&gt; &lt;consequent&gt; &lt;alternative&gt;)

条件表达式cond和predicate表达式（断言？也就是if）。这里有一个minor difference，cond的从句可以是多个表达式，但是if的从句必须是单一的表达式. =&gt; In an <strong>if</strong> expression, however, the &lt;consequent&gt; and &lt;alternative&gt; must be single expressions.

然后，就是布尔表达了，

(and (&gt; x 5) (&lt; x 10))

(define (&gt;= x y)

(or (&gt; x y) (= x y)))

(define (&gt;= x y)

(not (&lt; x y)))

<strong>7. Examples: Square Roots by Newton's Method</strong>

Newton's Method是一种通过连续的近似求值，不断趋近于平方根。Newton's Method实际上一种通用的求根方法，不止是针对平方根。在这里，求x的平方根方法是，首先，设定一个猜测值（guess）为1，然后，判断这个guess的平方，是否与x足够近似（good enough），如果不够，则改进guess，改进的guess的值是通过计算，( x/guess+guess ) / 2，这个表达式所得。

    (define (sqrt-iter guess x)
      (if (good-enough? guess x)
          guess
          (sqrt-iter (improve guess x)
                     x)))

    (define (improve guess x)
      (average guess (/ x guess)))

    (define (average x y)
      (/ (+ x y) 2))

    (define (good-enough? guess x)
      (&lt; (abs (- (square guess) x)) 0.001))

    (define (sqrt x)
      (sqrt-iter 1.0 x))

    (sqrt 9)

发现没有，这里没有loops（像for,while, do..while)。这里都是recursive，其中，我记得以前老师讲过，如果做recursive的话，是十分消耗efficiency的。因为，如果学过汇编就知道，在调用函数时，要将当前所有的变量，以及程序当前执行地址指针，压入到堆栈中。在结束函数调用时，就会从堆栈从一步一步返回到原来的执行点。这种操作显然是需要additional efficiency的。但是，在这本书page 25下面的注释写到，Readers who are worried about the efficiency issues involved in using procedure calls to implement iteration should note the remarks on "tail recursion" in section 1.2.1。这个tail recursion，我在学scala的时候，有看到过，也就是说，如果把递归函数的调用放在函数的最后，也就是返回值的位置，这样做，就不会需要additional efficiency。

<strong>8. Internal Definitions and Block Structure</strong>
<pre lang="scheme">(define (sqrt x)
  (define (good-enough? guess x)
    (&lt; (abs (- (square guess) x)) 0.001))
  (define (improve guess x)
    (average guess (/ x guess)))
  (define (sqrt-iter guess x)
    (if (good-enough? guess x)
        guess
        (sqrt-iter (improve guess x) x)))
  (sqrt-iter 1.0 x))</pre>
第一个程序和之前不同在于，作用域。good-enough?, improve, sqrt-iter，这三个procedures就限制在了sqrt这个过程中（block structure）。
<pre lang="scheme">(define (sqrt x)
  (define (good-enough? guess)
    (&lt; (abs (- (square guess) x)) 0.001))
  (define (improve guess)
    (average guess (/ x guess)))
  (define (sqrt-iter guess)
    (if (good-enough? guess)
        guess
        (sqrt-iter (improve guess))))
  (sqrt-iter 1.0))</pre>
第二个程序是稍作了简化，因为x的作用域可以覆盖这三个过程，所以就可以将x参数省略掉。

另外，good-enough?是对条件判断式的一种命名习惯。

第一次写，读书笔记，好像写了很多。但是，确实这本书，并不像书名，那样吓人。前面几章，应该还是很好理解的。读起来非常有意思，也学习到了许多概念。

多写，有助于自己消化阅读的内容。习题的解答，我会另写一个新的文章。