---
layout: post
title: "SICP Section 1.2"
category: [sicp]
tags: [sicp]
published: false
---
{% include JB/setup %}

<div id="_mcePaste">哎，昨天各种郁闷，网被关了，我去前台交钱，结果硬是说要online payment，我无语了，我房东每次都是在前台交的，轮到我就要online了，弄了半天，最终发现，住的地方换了网络服务商，房东因为回国的事，忘记交表格，申请新的套餐了。今天早上才去交，都不知道什么时候才能processing，我稍微看了一下邮件，估计得Jan 28th了。我倒是没什么，我那两个室友把房间都租出去了，新的租户估计要郁闷了。而且，今天在前台的时候，亲眼看到前台的人将一对印度人couple拦了下来，然后，问他们是住这里吗？那个女的说，是一个中国朋友的房子，然后，那个前台的人，说出于安全考虑，他们不能住这里，扣留了门卡。我无语了，这都行。。。不知道，我走后会不会发生这种事情，反正不管我的事情。</div>
<div id="_mcePaste">说到住的这个中介也是流氓，10月就问你要不要继续签约，签约就要签1年，我当时也没想太多，就觉得住这里还行，结果就签了1年，签到2012年2月。。。中介就是故意拖着你，明明知道12月，1月，2月，是放假的日子，就是让你不住也要交钱。还不准你租出去，各种流氓啊。</div>
<div id="_mcePaste">我要赶快熬过这段时间，回家休养一下，突然觉得身心疲惫。。。</div>
<div id="_mcePaste">遇到困难，郁闷的时候，就想想，这些东西都一定会过去的。。。</div>
<div id="_mcePaste"><strong>1. Linear Recursion and Iteration</strong></div>
<div id="_mcePaste">这两个词翻译过来，应该是线性递归和迭代吧。计算n！，这个是递归的老套路了，要求n！可以先求n-1！，一直下去，直到求到1，然后，结果一个一个返回来，就可以计算出n！。</div>
<div id="_mcePaste">Linear Recursion解决方案：</div>
<pre>(define (factorial n)
  (if (= n 1)
      1
      (* n (factorial (- n 1)))))</pre>
<div>对于，Linear Recursion，通过模拟它的计算，</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; white-space: pre-wrap; font-size: 12px; font-family: 'Courier New', Courier, 'Lucida Console', Monaco, 'DejaVu Sans Mono', 'Nimbus Mono L', 'Bitstream Vera Sans Mono'; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;">(factorial 6)
(* 6 (factorial 5))
(* 6 (* 5 (factorial 4)))
(* 6 (* 5 (* 4 (factorial 3))))
(* 6 (* 5 (* 4 (* 3 (factorial 2)))))
(* 6 (* 5 (* 4 (* 3 (* 2 (factorial 1))))))
(* 6 (* 5 (* 4 (* 3 (* 2 1)))))
(* 6 (* 5 (* 4 (* 3 2))))
(* 6 (* 5 (* 4 6)))
(* 6 (* 5 24)))
(* 6 120)
720</pre>
</div>
<div>可以得到，它的计算过程会建立一连串的延缓操作（deferred operations），在这个例子中，就是一连串的multiplication。而建立了这样一连串延缓操作的处理，也就叫递归处理（recursive process）。</div>
<div>Iteration解决方案：</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; white-space: pre-wrap; font-size: 12px; font-family: 'Courier New', Courier, 'Lucida Console', Monaco, 'DejaVu Sans Mono', 'Nimbus Mono L', 'Bitstream Vera Sans Mono'; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;">(define (factorial n)
  (fact-iter 1 1 n))

(define (fact-iter product counter max-count)
  (if (&gt; counter max-count)
      product
      (fact-iter (* counter product)
                 (+ counter 1)
                 max-count)))</pre>
</div>
<div>用迭代的方法的话，就稍微复杂一点，fact-iter维护三个变量，product（乘积），counter（计数器），max-count（计数器的最大值）。通过counter和调用fact-iter，完成迭代的过程。</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; white-space: pre-wrap; font-size: 12px; font-family: 'Courier New', Courier, 'Lucida Console', Monaco, 'DejaVu Sans Mono', 'Nimbus Mono L', 'Bitstream Vera Sans Mono'; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;">(factorial 6)
(fact-iter   1 1 6)
(fact-iter   1 2 6)
(fact-iter   2 3 6)
(fact-iter   6 4 6)
(fact-iter  24 5 6)
(fact-iter 120 6 6)
(fact-iter 720 7 6)
720</pre>
</div>
<div id="_mcePaste">对于迭代过程，它有固定数量的状态变量（state variables），迭代设定了这些状态变量如何变化，以及在什么情况下终止。</div>
<div><strong>2. 区别recursive process和recursive procedure</strong></div>
<div>recursive procedure的概念就是直接地或者间接地 ”自己调用自己“。 而recursive process是指处理的过程是递归形式的，如同上面所讲，有一连串的延缓操作（deferred operations）。然后，就是可以通过上面迭代的例子看到，实际上fact-iter是一个recursive procedure，但是，它在计算的时候，是通过iterative process，书中如此解释 ==&gt; However, the process really is iterative: Its state is captured completely by its three state variables, and an interpreter need keep track of only three variables in order to execute the process。</div>
<div>另一个，之所以会把process和procedure弄混淆的原因是，在很多常见的语言中（例如，Ada，Pascal，C），在处理recursive procedure的时候，不管它是recursive process，还是iterative process，都会随着procedure calls的增加，而增加相应的内存消耗。所以，这些语言只能运用”looping constructs“来描述iterative process，例如for，while，do。而在Scheme中，即使是在recursive procedure中，执行iterative process，也只会消耗固定的内存。这种实现的方法就叫尾部递归（tail-recursive），通过tail-recursive，iteration就可以转换成原来的过程调用机制。</div>
<div><strong>3. Tree Recursion</strong></div>
<div>另一种常见的计算模式叫作树形递归（Tree Recursion），一个例子就是计算Fibonacci numbers, Fib(0) = 0, Fib(1)= 1, Fib(n) = Fib(n-1) + Fib(n-2)。</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; white-space: pre-wrap; font-size: 12px; font-family: 'Courier New', Courier, 'Lucida Console', Monaco, 'DejaVu Sans Mono', 'Nimbus Mono L', 'Bitstream Vera Sans Mono'; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;">(define (fib n)
  (cond ((= n 0) 0)
        ((= n 1) 1)
        (else (+ (fib (- n 1))
                 (fib (- n 2))))))</pre>
</div>
<div>例如计算（fib 5），我们可以计算（fib 4）和（fib 3），计算（fib 4），我们可以计算（fib 3）和（fib 2）。这样，就形成了一种树形结构的计算，就像二叉树一样。这样就表明，fib在每次调用自身的时候，会调用2次。</div>
<div>还有一个有趣的地方，就是Fibonacci和golden ratio有一定的关系，Fib(n)是一个最接近<span style="color: #333333; font-family: Verdana, Geneva, Arial, Helvetica, sans-serif; line-height: 24px; font-size: 14px;">$$\phi ^{n} / \sqrt{5}$$</span>的整数，</div>
<div>而$$\phi = (1 + \sqrt{5})/2 \approx 1.6180$$，$$\phi$$就是黄金比例，它满足$$\phi^{2}=\phi+1$$。</div>
<div>计算Fibonacci的iterative process，如下</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; white-space: pre-wrap; font-size: 12px; font-family: 'Courier New', Courier, 'Lucida Console', Monaco, 'DejaVu Sans Mono', 'Nimbus Mono L', 'Bitstream Vera Sans Mono'; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;">(define (fib n)
  (fib-iter 1 0 n))

(define (fib-iter a b count)
  (if (= count 0)
      b
      (fib-iter (+ a b) a (- count 1))))</pre>
</div>
<div><strong>4. Example: Counting change</strong></div>
<div>这是一个相当于找零钱的例子，给你1dollar，根据现有的硬币种类，计算出有多少种不同的方法，这里有一种简单的递归解决方案。</div>
<div>首先，假定硬币的种类按照一定的顺序排列，那么，将a dollars用n种硬币来描述的方法等于，</div>
<div>
<ul>
	<li>不使用第一种硬币，用其他硬币描述a的方法数量</li>
	<li>用所有的硬币种类来描述a - d，d是第一种硬币的价值</li>
</ul>
</div>
<div>以上这两个数量加起来，就等于用n种硬币描述a dollars的方法总数。</div>
<div>然后，我们制定退化情况（degenerate cases）：</div>
<div>
<ul>
	<li>如果a等于0，那么，只有一种方法可以找零</li>
	<li>如果a小于0，那么，没有方法可以找零</li>
	<li>如果n等于0，那么，没有方法可以找零</li>
</ul>
</div>
<div>代码如下：</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; white-space: pre-wrap; font-size: 12px; font-family: 'Courier New', Courier, 'Lucida Console', Monaco, 'DejaVu Sans Mono', 'Nimbus Mono L', 'Bitstream Vera Sans Mono'; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;">(define (count-change amount)
  (cc amount 5))

(define (cc amount kinds-of-coins)
  (cond ((= amount 0) 1)
        ((or (&lt; amount 0) (= kinds-of-coins 0)) 0)
        (else (+ (cc amount
                     (- kinds-of-coins 1))
                 (cc (- amount
                        (first-denomination kinds-of-coins))
                     kinds-of-coins)))))

(define (first-denomination kinds-of-coins)
  (cond ((= kinds-of-coins 1) 1)
        ((= kinds-of-coins 2) 5)
        ((= kinds-of-coins 3) 10)
        ((= kinds-of-coins 4) 25)
        ((= kinds-of-coins 5) 50)))</pre>
</div>
<div><strong>5. Orders of Growth</strong></div>
<div>这章的1.2.3，介绍了算法复杂度的增长率的问题，这些问题可以看《数据结构与算法分析——C语言描述》这本书，讲的非常清楚。</div>
<div><strong>6. Exponentiation</strong></div>
<div>求幂，也就是给一个基数b，一个正整数幂n，求b的n次方。一种方法通过递归的定义，下面是recursive process的代码，</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; white-space: pre-wrap; font-size: 12px; font-family: 'Courier New', Courier, 'Lucida Console', Monaco, 'DejaVu Sans Mono', 'Nimbus Mono L', 'Bitstream Vera Sans Mono'; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;">(define (expt b n)
  (if (= n 0)
      1
      (* b (expt b (- n 1)))))</pre>
</div>
<div>然后，是iterative process的代码，</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; white-space: pre-wrap; font-size: 12px; font-family: 'Courier New', Courier, 'Lucida Console', Monaco, 'DejaVu Sans Mono', 'Nimbus Mono L', 'Bitstream Vera Sans Mono'; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;">(define (expt b n)
  (expt-iter b n 1))

(define (expt-iter b counter product)
  (if (= counter 0)
      product
      (expt-iter b
                 (- counter 1)
                 (* b product))))</pre>
</div>
<div>上面这两种方法，都是以“每次乘以一个b”为基础，其实，我们也通过不断的平方，来求幂，例如，b的2次方等于b * b, b的4次方等于2个b的2次方相乘，b的8次方等于2个b的4次方相乘。我们可以比较，</div>
<div>b * (b * (b * (b * (b * (b * (b * b))))))</div>
<div>而通过不断平方求幂，只需要3次相乘就可以完成。下面是通过不断平方，求幂的代码：</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; white-space: pre-wrap; font-size: 12px; font-family: 'Courier New', Courier, 'Lucida Console', Monaco, 'DejaVu Sans Mono', 'Nimbus Mono L', 'Bitstream Vera Sans Mono'; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;">(define (fast-expt b n)
  (cond ((= n 0) 1)
        ((even? n) (square (fast-expt b (/ n 2))))
        (else (* b (fast-expt b (- n 1))))))

(define (even? n)
  (= (remainder n 2) 0))</pre>
</div>
<div>这个算法的复杂度是O(logn)。</div>
<div><strong>7. Greatest Common Divisors</strong></div>
<div>这个就是找出，可以整除2个数的，最大值。例如，16和28的GCD就是4。这里有一个著名的算法来解决这个问题，r是a除以b的余数，a和b的公共除数等于b和r的公共除数。因此，</div>
<div>GCD（a，b）= GCD（b，r）</div>
<div>通过辗转相除，简化问题，</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; white-space: pre-wrap; font-size: 12px; font-family: 'Courier New', Courier, 'Lucida Console', Monaco, 'DejaVu Sans Mono', 'Nimbus Mono L', 'Bitstream Vera Sans Mono'; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;"><span style="font-family: 'Segoe UI', Calibri, 'Myriad Pro', Myriad, 'Trebuchet MS', Helvetica, Arial, sans-serif; line-height: 19px; white-space: normal; font-size: 13px;">
<div style="padding: 0px; margin: 0px;">GCD（204,40）=GCD（40,6）</div>
<div style="padding: 0px; margin: 0px;">                      =GCD（6,4）</div>
<div style="padding: 0px; margin: 0px;">                      =GCD（4,2）</div>
<div style="padding: 0px; margin: 0px;">                      =GCD（2,0）</div>
<div style="padding: 0px; margin: 0px;">                      = 2</div>
</span></pre>
</div>
<div>这个算法其实我在就有接触，记得还是在大一学数据结构的时候，我自学《数据结构与算法分析——C语言描述》这本书的时候，里面就讲到了这个算法，当时，我觉得十分惊讶，因为这个算法太精妙了，还把这个算法的C语言版本，写在了原来的新浪的博客上。下面是Scheme的代码，</div>
<pre>(define (gcd a b)
  (if (= a b)
      a
      (gcd b (remainder a b))))</pre>
<div>在分析这个算法的复杂度时，用到了一个理论：</div>
<blockquote>
<div><span style="font-style: normal;"><strong>Lame's Theorem</strong>: If Euclid's Algorithm requires k steps to compute the GCD of some pair, then the smaller number in the pair must be greater than or equal to the kth Fibonacci number.</span></div></blockquote>
<div>也就是说，计算GCD时，如果计算了k次，那么这对pair中最小的那个数，就大于或者等于Fibonacci里面的第k个数字。假设n是那个最小的数，计算了k次，而$$n \geq Fib(k) \approx \phi^{k} / \sqrt{5}$$，所以$$k=\log_{\phi}n$$，因此，这个算法的时间复杂度等于O(logn）。</div>
<div><strong>8. Example: Testing for Primality</strong></div>
<div>素性测试，也就是判断一个整数n是不是质数。这章，给出了2个算法，一个算法的时间复杂度是，n的开方，另一个的时间复杂度是logn。</div>
<div>第一个方法，要判断一个数是不是质数，最简单的方法就是找它的除数，如果这个数的除数，只是1和它自己本身，那么它就是质数。（我这个学期Java的最后一道题，就是要你用Java写出这个程序，可惜啊，没早看这本书，好像写错了一点点。）</div>
<div>这个方法，就是从2开始，一直到n的开方，如果找到了，那么就不是质数，反之，如果没找到，那么它就是质数。</div>
<div>
<pre style="margin-top: 1em; margin-right: 0px; margin-bottom: 1em; margin-left: 0px; font: normal normal normal 12px/18px Consolas, Monaco, 'Courier New', Courier, monospace; overflow-x: hidden; overflow-y: hidden; background-color: #ffffff; width: 588px; padding: 0.8em; border: 1px solid #dddddd;">(define (smallest-divisor n)
  (find-divisor n 2))

(define (find-divisor n test-divisor)
  (cond ((&gt; (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (+ test-divisor 1)))))

(define (divides? a b)
  (= (remainder b a) 0))

(define (prime? n)
  (= n (smallest-divisor n)))</pre>
</div>
<div>这个是最基本的方法，也是大家通常能想到的方法。</div>
<div>而第二个方法叫，The Fermat test，这个算法的复杂度在O(logn)，它主要基于数论的一个结果：</div>
<blockquote>
<div><span style="font-style: normal;"><strong>Fermat's Little Theorem</strong>: If <strong>n</strong> is a prime number and <strong>a</strong> is any positive integer less than <strong>n</strong>, then <strong>a</strong> raised to the <strong>n</strong>th power is congruent to <strong>a</strong> module <strong>n</strong>.</span></div></blockquote>
<div>也就是说，如果n是一个质数，a是小于n的任意一个正整数，那么a的n次方与n取模，如果不等于a，那么n肯定不是质数，如果等于a，它有可能是质数。</div>
<div>通过这样一个理论，就可以产生素性测试的算法：给定一个n，随机选择一个a&lt;n，然后，计算a的n次方与n取模：</div>
<div>
<ul>
	<li>不等于a，那么n肯定不是质数，</li>
	<li>等于a，那么n有可能是质数。</li>
</ul>
</div>
<div>通过不断的测试（改变a的值），我们可以提高这个结果的可信度。</div>
<div>为了实现Fermat test，我们先要实现一个求幂取模的过程，</div>
<pre>(define (expmod base exp m)
  (cond ((= exp 0) 1)
        ((even? exp)
         (remainder (square (expmod base (/ exp 2) m))
                    m))
        (else
         (remainder (* base (expmod base (- exp 1) m))
                    m))))</pre>
<div>然后，</div>
<pre>(define (fermat-test n)
  (define (try-it a)
    (= (expmod a n n) a))
  (try-it (+ 1 (random (- n 1)))))

(define (fast-prime? n times)
  (cond ((= times 0) true)
        ((fermat-test n) (fast-prime? n (- times 1)))
        (else false)))</pre>
<div>try-it过程就是测试求幂取模后，与a是否相等。fast-prime?的两个参数，n是要测试的数，times是要测试几次。cond的条件如下：</div>
<div>
<ul>
	<li>如果times等于0，那么代表测试的次数完成了，这个数就是质数，</li>
	<li>如果fermat-test的结果为true，那么代表，这个数有可能是质数，继续测试，（由于可以进入到这个条件，代表times还不是0）</li>
	<li>如果上面的情况都不是，代表fermat-test返回的结果为false，这个数不是质数。</li>
</ul>
</div>
<div>上面这种算法，是一种概率的方法（probabilistic methods），通过随机抽取，判断这个数的性质，抽取的次数越多，结果的可信度就越高。</div>
<div>但是，也有一些数，它们不是质数，但是却可以通过Fermat test，它们叫作Carmichael numbers，但是非常的少，在100,000,000以下，只有255个，例如561，1105，1729，2465，2821和6601。</div>
<div>好啦，1.2节的内容就完成了，好多内容啊，其实早就应该写好了，但是，断网，把心情弄没了。练习题还有很多呢，明天写，继续加油。</div>