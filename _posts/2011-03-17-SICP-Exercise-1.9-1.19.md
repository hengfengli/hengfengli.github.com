---
layout: post
title: "SICP Exercise 1.9-1.19"
categories: [sicp]
tags: [sicp]
published: false
---
{% include JB/setup %}

<div><strong>Exercise 1.9</strong></div>
<div>两个实现加法的过程，一个是recursive process，一个是iterative process。</div>
<div>（1）recursive process</div>
<pre>(define (+ a b)
  (if (= a 0)
      b
      (inc (+ (dec a) b))))</pre>
<div>它的计算过程：</div>
<pre>(inc (+ (3 5)))
(inc (inc (+ 2 5)))
(inc (inc (inc (+ 1 5))))
(inc (inc (inc (inc (inc (+ 0 5))))))
(inc (inc (inc (inc 5))))
(inc (inc (inc 6)))
(inc (inc 7))
(inc 8 )
9</pre>
<div>（2）iterative process</div>
<pre>(define (+ a b)
  (if (= a 0)
      b
      (+ (dec a) (inc b))))</pre>
<div>它的计算过程：</div>
<pre>(+ 4 5 )
(+ 3 6 )
(+ 2 7 )
(+ 1 8 )
(+ 0 9 )</pre>
<div><strong>Exercise 1.10</strong></div>
<div>题目中，给了一个计算Ackermann‘s函数的过程，</div>
<pre>
<div id="_mcePaste">(define (A x y)
  (cond ((= y 0) 0)
        ((= x 0) (* 2 y))
        ((= y 1) 2)
        (else (A (- x 1)
                 (A x (- y 1))))))</div></pre>
<div>第一个问题，计算三个值：</div>
<pre>(A 1 10)
(A 0 (A 1 9))
(A 0 (A 0 (A 1 8)))
(A 0 (A 0 (A 0 =&gt; 10 times
So, (A 1 10)=2^10=1024

(A 2 4)
(A 1 (A 2 3))
(A 1 (A 1 (A 2 2)))
(A 1 (A 1 (A 1 (A 2 1))))
We see that (A 1 x)=2^x
So, (A 2 4)=2^(2^(2^2))=2^16=65536

(A 3 3)
(A 2 (A 3 2))
(A 2 (A 2 (A 3 1)))
(A 2 (A 2 2))
So (A 3 3)=(A 2 4)=2^16=65536</pre>
<div>第二个问题，</div>
<div>Consider the following procedures, where A is the procedure defined above:</div>
<pre>(define (f n) (A 0 n))
(define (g n) (A 1 n))
(define (h n) (A 2 n))
(define (k n) (* 5 n n))</pre>
<div>Give concise mathematical definitions for the functions computed by the procedures f, g, and h for positive integer values of n. For example, (k n) computes 5n^2.</div>
<div>也就是说，用n的数学式子，来表达这个函数，f(n)=2n, g(n)=2^n, h(n)=2^(2^(2^(... 2) (n times)，这个Ackermann函数的增长速度非常快，到(A 4 3)已经大得无法精准计算了。</div>
<div><strong>Exercise 1.11</strong></div>
<div>首先，把题目定义的函数用recursive process表达出来，因为这种方式一般是最简单的。</div>
<pre>(define (f n)
  (cond ((&lt; n 3) n)
        (else (+ (f (- n 1))
                 (* 2 (f (- n 2)))
                 (* 3 (f (- n 3)))))))</pre>
<div>然后，通过跟计算Fibonacci一样的，要把它转换成iterative的形式，就要维护几个状态变量，就像一个队列一样，a始终是最新计算出来的值。</div>
<pre>a &lt;- a + 2b + 3c
b &lt;- a
c &lt;- b</pre>
<pre>(define (f n)
  (if (&lt; n 3)
      n
      (f-iter 2 1 0 n)))

(define (f-iter a b c count)
  (if (= count 2)
      a
      (f-iter (+ a (* 2 b) (* 3 c))
              a
              b
              (- count 1))))</pre>
<div>例如，当n=3的时候，(f-iter 2 1 0 3)，因为count！=2，那么就会计算一次，a的值就是上一次计算的值。</div>
<div><strong>Exercise 1.12</strong></div>
<div>用递归的方式，计算杨辉三角，也叫Pascal's triangle。</div>
<pre>(define (pascal row col)
  (cond ((= row 1) 1)
        ((= row col) 1)
        ((= col 1) 1)
        (else (+ (pascal (- row 1) col)
                 (pascal (- row 1) (- col 1))))))</pre>
<div><strong>Exercise 1.13</strong></div>
<div>题目要求证明Fib(n)是最接近$$\phi^{n}/\sqrt{5}$$的整数，而$$\phi=(1+\sqrt{5})/2$$。然后，题目给了hints，说让$$\psi=(1-\sqrt{5})/2$$，然后证明$$Fib(n)=(\phi^{n}-\psi^{n})/\sqrt{5}$$。</div>
<div>首先，条件</div>
<div>$$\phi =(1+\sqrt{5})/2 $$</div>
<div>$$\psi=(1-\sqrt{5})/2 $$</div>
<div>假设，$$Fib(n) = \frac{ \phi^{n} - \psi^{n}}{\sqrt{5}}$$成立，</div>
<div>通过fibonacci的定义推导，</div>
<div>$$Fib(n) = Fib(n-1) + Fib(n-2) = \frac{(\phi^{n-1}-\psi^{n-1})}{\sqrt{5}}+\frac{(\phi^{n-2}-\psi^{n-2})}{\sqrt{5}}$$</div>
$$=\frac{      (\phi+1)     \phi^{n-2}   - (\psi+1)   \psi^{n-2}           }{\sqrt{5}}$$

因为，$$\phi^{2}=\phi+1$$，$$\psi^{2}=\psi+1$$

所以，$$Fib(n)=\frac{\phi^{n}-\psi^{n}             }{\sqrt{5}}$$，符合fibonacci的定义。
<div><strong>Exercise 1.14</strong></div>
<div>第一个问题要求，画出count-change对于11cents找零的计算过程，也是树形，和fibonacci的差不多。</div>
<div>第二个问题要求，分析空间（space）和处理的步数（number of steps），space是线性增长，而steps是polynomial增长。</div>
<div><strong>Exercise 1.15</strong></div>
<div>当$$x$$足够小的时候，$$\sin{x} \approx x$$，通过这样来计算$$\sin{x}$$的值。而通过$$\sin{x} = 3\sin{\frac{x}{3}}-4\sin^3{\frac{x}{3}}$$，来不断减小$$x$$的大小，直到满足$$x$$很小的时候，可以替代$$\sin{x}$$的值。</div>
<div>程序代码如下：</div>

    (define (cube x) (* x x x))

    (define (p x) (- (* 3 x) (* 4 (cube x))))

    (define (sine angle)
        (if (not (&gt; (abs angle) 0.1))
            angle
            (p (sine (/ angle 3.0)))))


<div>可以看到，通过不断地递归调用，减小angle的值，直到满足足够小的条件（在这里是0.1），然后，再通过（deferred operations，在这里也就是p过程），一步步计算出最初$$\sin{x}$$的值。</div>
<div>问题一，如果计算（sine 12.15），p过程会调用多少次？</div>
<div>过程如下，</div>
<pre>(sine 12.15)
(p (sine 4.05))
(p (p (sine 1.35)))
(p (p (p (sine 0.45))))
(p (p (p (p (sine 0.15)))))
(p (p (p (p (p (sine 0.05))))))</pre>
<div>所以，p过程的调用次数为5次。</div>
<div>问题二，还是关于space和number of steps，可以看到，都是对数级的，$$O(\log_{3}{angle})$$。</div>
<div><strong>Exercise 1.16</strong></div>
<div>用iterative process写fast-expt，代码如下，</div>
<pre>(define (fast-expt x n)
  (expt-iter 1 x n))

(define (expt-iter a b n)
  (cond ((= n 0) a)
        ((even? n) (expt-iter a (* b b) (/ n 2)))
        (else (expt-iter (* a b) b (- n 1)))))

(define (even? n)
  (= (remainder n 2) 0))</pre>
<div>利用的是这样一个公式$$(b^{n/2})^{2}=(b^{2})^{n/2}$$，每次迭代维护三个变量a，b，n，每次迭代后，保持$$ab^{n}$$不变。例如，求$$3^{10}$$，大概的过程如下</div>
<pre>(fast-expt 3 10)
(expt-iter 1 3 10)
(expt-iter 1 9 5)
(expt-iter 9 9 4)
(expt-iter 9 81 2)
(expt-iter 9 81*81 1)
(expt-iter 9*81*81 81*81 0)</pre>
<div>当为偶数的时候，就将基数平方，n除以2。如果为奇数的话，就将基数乘以a，并将结果存到a中，基数不变，n减1。就像这样$$3^{10}=(3^{2})^{5}=(3^2)(3^{2})^{4}=(3^2)(3^{4})^{2}=(3^2)(3^{8})^{1}=(3^{2}\times 3^{8})(3^{8})^{0}$$，这个式子就是$$ab^{n}$$，它的值总是一样的，当n=0的时候，计算结束，a就是最终的结果。</div>
<div><strong>Exercise 1.17</strong></div>
<div>题目中，给出了一个实现整数乘法，通过不断的加法递归实现，代码如下</div>
<pre>(define (* a b)
  (if (= b 0)
      0
      (+ a (* a (- b 1)))))</pre>
<div>上面这个算法的时间复杂度跟b的大小成线性比，也就是O(b)。那么，现在要求写出一个跟fast-expr一样，时间复杂度为log的算法，代码如下，</div>
<pre>(define (multi a b)
  (cond ((= b 0) 0)
        ((even? b) (double (multi a (halve b))))
        (else (+ a (multi a (- b 1))))))</pre>
<div><strong>Exercise 1.18</strong></div>
<div>用iterative process实现1.17中的算法，</div>
<pre>(define (multi a b)
  (multi-iter 0 a b))

(define (multi-iter s a b)
  (cond ((= b 0) s)
        ((even? b) (multi-iter s (double a) (halve b)))
        (else (multi-iter (+ s a) a (- b 1)))))</pre>
<div>例如，求3*10，$$3\times 10=0+(3\times 2) \times 5=(3\times 2) + (3\times 2)\times 4$$，遇到偶数就除以2，遇到奇数就提取出来一个基数，使得下一次计算变成偶数，可以整除2。</div>
<div><strong>Exercise 1.19</strong></div>
<div>这个题目提出了一个log计算fibonacci的方法。首先，我们在之前用iterative计算fibonacci的时候，利用了这样一个transformation：</div>
<div>a &lt;- a + b</div>
<div>b &lt;- a</div>
<div>现在，我们把这个过程叫做T，从（1,0）开始，通过T的n次方，最终得到结果。</div>
<div>现在，假设$$T_{pq}$$是一个转换关系，它将（a，b），根据以下式子转换</div>
<div>a &lt;- bq + aq + ap</div>
<div>b &lt;- bp +aq</div>
<div>而T是，当p=0,q=1时，$$T_{pq}$$的特殊情况。</div>
<div>现在我们发现一个情况，计算$$T_{pq}$$2次的结果，等于计算$$T_{p'q'}$$1次的结果。这样，我们就能像fast-expt那样通过log次计算步骤，得到结果。</div>
<div>题目现在要求你找出p'和q'与p和q的关系。</div>
<div>a' &lt;- bq + aq + ap</div>
<div>b' &lt;- bp + aq</div>
<div>a'' &lt;- b'q + a'q + a'p</div>
<div>b'' &lt;- b'p + a'q</div>
<div>将a’和b‘代入到a’‘和b’‘的计算中，最终可以得到，</div>
<div>$$a'' \leftarrow{b(q^{2}+2pq)} + a(q^{2}+2pq)+a(q^{2}+p^{2})$$</div>
<div>$$b'' \leftarrow{b(p^{2}+ q^{2})} + a(q^{2}+2pq)$$</div>
<div>这样，可以得到</div>
<div>$$p'=p^{2}+q^{2}$$</div>
<div>$$q'=q^{2}+2pq$$</div>
<div>得到了关系，我们也就可以将题目中的代码完成了，</div>
<pre>(define (fib n)
  (fib-iter 1 0 0 1 n))

(define (fib-iter a b p q count)
  (cond ((= count 0) b)
        ((even? count)
         (fib-iter a
                   b
                   (+ (* p p) (* q q))
                   (+ (* q q) (* 2 p q))
                   (/ count 2)))
        (else (fib-iter (+ (* b q) (* a q) (* a p))
                        (+ (* b p) (* a q))
                        p
                        q
                        (- count 1)))))</pre>