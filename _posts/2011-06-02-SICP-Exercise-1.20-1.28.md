---
layout: post
title: "SICP Exercise 1.20-1.28"
category: [sicp]
tags: [sicp]
published: false
---
{% include JB/setup %}

<div>

<strong>Exercise 1.20</strong>

这个题目，还是关于Section 1.1里讲到的Applicative Order versus Normal Order。要你计算，在不同的Order下，gcd中reminder的调用次数。

</div>
<pre>(gcd 206 40)
(gcd 40 (remainder 206 40))
(gcd 40 6)
(gcd 6 (remainder 40 6))
(gcd 6 4)
(gcd 4 (remainder 6 4))
(gcd 4 2)
(gcd 2 (remainder 4 2))
(gcd 2 0)
2</pre>
<div>上面这个就是Applicative Order，也就是先调用函数计算出结果，将结果作为参数再进行计算，remainder一共被调用了４次。如果按照Normal Order，那就复杂的多了，remainder先不会进行计算，先将gcd全部展开。非常复杂，我就不在这里写全了，自己可以试试。</div>
<pre>(gcd 206 40)
(gcd 40 (remainder 206 40))

(if (= (remainder 206 40) 0) ; 1 time
    40
    (gcd (remainder 206 40)
         (remainder 40 (remainder 206 40))))</pre>
<div>从上面就可以看到，(remainder 206 40) 作为整体带入gcd中了。</div>
<div><strong>Exercise 1.21</strong></div>
<div>用smallest-divisor来计算，199，1999，19999的最小除数，分别为199, 1999, 7, 也就是说19999不是质数。</div>
<div><strong>Exercise 1.22</strong></div>
<div>题目要求用下面这个timed-prime-test来测试时间。</div>
<pre>(define (timed-prime-test n)
  (newline)
  (display n)
  (start-prime-test n (runtime)))

(define (start-prime-test n start-time)
  (if (prime? n)
      (report-prime (- (runtime) start-time))))

(define (report-prime elapsed-time)
  (display " *** ")
  (display elapsed-time))</pre>
<div>其中，(runtime)是代表系统的当前时间，然后，需要你写出一个search-for-primes来计算一定范围内的primes。</div>
<pre>(define (display-prime n)
  (newline)
  (display n)
  (display " is a prime"))

(define (even? n)
  (= (remainder n 2) 0))

(define (search-for-primes start end)
  (let ((start (if (even? start) (+ start 1) start)))
    (do ((i start (+ i 2)))
        ((&gt; i end))
        (if (prime? i) (display-prime i)))))</pre>
<div><strong>Exercise 1.23</strong></div>
<pre>(define (next n)
  (if (= n 2)
      3
      (+ n 2)))</pre>
<div>题目也就是说，原先的查找divisors的方法，是从2，3，4，5，6...这样下去的，但由于偶数肯定不是，所以我们可以这样，如果是2的话，下一个就是3，如果不是2的话，就加2。这样，将测试的步骤减半，加快查找速度。</div>
<div><strong>Exercise 1.24</strong></div>
<div>题目说，用fast-prime?替换这种迭代式的测试查找。我想，由于fast-prime?的测试次数都是由自己定，所以无论数字多大，花费的时间应该都是相等的。</div>
<div><strong>Exercise 1.25</strong></div>
<div>这个问题是说，我们已经知道了fast-expt的方法，为什么我们不用它直接算出base的exp次方，然后再和m取模？原因是，如果先算出答案，fast-expt的答案将会巨大无比，int一般是32位bits，所以无法计算超出这个的数字。而作者原来的方法，就不存在这个问题，因为它每次计算完之后 ，都与m取模，使得结果不会变得很大。</div>
<div><strong>Exercise 1.26</strong></div>
<div>这个题目中，Louis没有用square函数，而是直接写了两遍expmod方法，这样就导致原来的O(logn)变成了O(n)，因为2^logn=n。</div>
<div><strong>Exercise 1.27</strong></div>
<div>有一群数字，可以说是这个Fermat test的bug，也就是说，它们可以糊弄这个测试，这里我想到了2个概念false positive和false negative，这两个概念，我一直没弄清楚是啥，直到我前几天看的一篇文章，才明白。</div>
<blockquote>
<div>"Basically, a false positive is a data instance where the model we've created predicts it should be positive, but instead, the actual value is negative. Conversely, a false negative is a data instance where the model predicts it should be negative, but the actual value is positive."</div></blockquote>
<div>也就是说，false positive是我们的model(这里指这个test)判断它应该为正确的，但实际上，这个结果是错误的。而false negative就是我们的model判断它应该为错误的，但是，这个结果实际上是正确的。那么在实际的应用中，这两个概念又有什么用呢？从我看的那篇文章里，举了这样一个例子，在一个检测垃圾邮件的系统中，哪个更重要呢？false positive意味着，将垃圾邮件作为了正常的邮件。而false negative意味着，将正常的邮件判断为垃圾邮件。那么，由此看来，false negative应该更重要一些，因为我们不希望漏掉正常的邮件，偶尔出现垃圾邮件倒是无所谓。</div>
<div>而这里的Carmichael numbers就是false positive，Fermat test认为它们是prime，但实际上，它们不是。例如，书中给出的，561，1105，1729，2465，2821，6601，在100,000,000里面有255个这样的数字。</div>
<div><strong>Exercise 1.28</strong></div>
<div>上一道题里，说了存在这样一些数字，可以糊弄Fermat test。那么，有什么方法可以避免呢？这个叫Miller-Rabin test，取一个随机数a &lt; n，在我们将a提升至$$a^{n-1}$$的过程中，如果我们发现，一个数与n取模等于1，但这个数不是1和n-1，那么n就不是质数。</div>
<pre>#lang scheme
(define (square x) (* x x))

(define (expmod base exp m)

  (cond ((= exp 0) 1)
        ((even? exp)
         (let* ( (cand (expmod base(/ exp 2) m))
                (root (remainder (square cand) m)))
           (if (and (not (= cand 1)) (not (= cand (- m 1))) (= root 1))
               0
               root)))
        (else
         (remainder (* base (expmod base (- exp 1) m))
                    m))))

(define (miller-rabin-test n)
  (define (try-it a)
    (= (expmod a (- n 1) n) 1))
  (try-it (+ 1 (random (- n 1)))))

(define (fast-prime? n times)
  (cond ((= times 0) true)
        ((miller-rabin-test n) (fast-prime? n (- times 1)))
        (else false)))

(fast-prime? 1105 3)</pre>
<div>可以看到，在计算$$a^{n-1}$$的过程中，会不停地检测，存不存在可以与n取模等于1的数字。</div>