---
layout: post
title: "[HL-8] Dynamic programming"
categories: 
    - algorithm
tags: 
    - dp
published: true
---

I just solved two similar Leetcode problems, 
[010 Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/) 
and 
[044 Wildcard Matching](https://leetcode.com/problems/wildcard-matching/), 
both of which can be addressed by **dynamic programming technique**. These two
problems are not easy to solve. I couldn't solve 010 until I referenced
someone's solution. Then, I applied the same idea to 044. But I found that 044
is easier than 010. Thus, we start from 044 Wildcard Matching. 



# Wildcard Matching

* Implement wildcard pattern matching with support for '?' and '\*'. 

~~~python
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
~~~

For such problem, I always think about the dynamic programming technique to
compute the **edit  distance** that I learned before. 

Assume that '\$' means an empty character added to the starting position of
**s** and **p**. I play a little trick by adding '\$' to **s** and **p** for
convenient indexing. So the table T[i][j] represent **whether or not s[:i+1]
match to p[:j+1]**, True or False. T[0][0] compares two empty strings, which is
definitely true: 

![](/assets/img/hl-8-dp-0.png)

Then, consider to compare a non-empty text to an empty pattern, which is 
definitely false. 

![](/assets/img/hl-8-dp-1.png)

Consider to compare an empty text to a non-empty pattern, which can only be true
when pattern is "\*...\*". When p[j] is '\*', it can only match 0 characters
to an empty text. Thus, it follows the previous answer without the current
character '\*' in the pattern. 

![](/assets/img/hl-8-dp-2.png)

* T[i][j] = T[i-1][j-1] when s[i] == p[j] or p[j] == '?', 
e.g., T[1][1] = T[0][0] and T[2][1] = T[1][0]
* If p[j] is '\*', it can match 0 characters to the text: T[i][j-1]. Or it can
match at least 1 character to the text: T[i-1][j]. T[1][2] = T[1][1] (True) or
T[0][2] (False). 

![](/assets/img/hl-8-dp-3.png)

Finally, for '\*' in the pattern, its result depends on two options: matching 
0 or 1 characters to the text. T[2][2] = T[2][1] (False) or T[1][2] (True). 

![](/assets/img/hl-8-dp-4.png)

~~~python
class Solution(object):
    
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        s = '$' + s
        p = '$' + p
        n, m = len(s), len(p)
        
        # Learned from this following post: 
        # leetcode.com/discuss/75670/python-dp-solution-avoid-tle-o-m-n-time-o-n-space
        # After removing stars, if the pattern is longer than the text, 
        # return False, :-)
        if m - p.count('*') > n:   #avoid TLE
            return False
        
        table = [[False]*(m) for _ in range(n)]
        
        table[0][0] = True
        
        # empty string to non-empty pattern
        for j in range(1, m):
            if p[j] == '*':
                table[0][j] = table[0][j-1]
        
        for i in range(1, n):
            for j in range(1, m):
                
                if s[i] == p[j] or p[j] == '?':
                    table[i][j] = table[i-1][j-1]
                elif p[j] == '*':
                    # match 0 chars in t, skip a char in the pattern
                    # match at least 1 char in t
                    table[i][j] = table[i][j-1] or table[i-1][j]
        
        return table[n-1][m-1]

sol = Solution()
assert sol.isMatch("aa","a") == False
assert sol.isMatch("aa","aa") == True
assert sol.isMatch("aaa","aa") == False
assert sol.isMatch("aa", "*") == True
assert sol.isMatch("aa", "a*") == True
assert sol.isMatch("ab", "?*") == True
assert sol.isMatch("aab", "c*a*b") == False
~~~

# Regular Expression Matching

This question is much difficult than the above one, because the pattern is 
more complex. 

* Implement regular expression matching with support for '.' and '\*'.

~~~python
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
~~~

I learned the idea from [this post](https://leetcode.com/discuss/95803/python-dp-solution-with-detailed-comments). 
Similar idea to Wildcard Matching. We start from the following case: 

![](/assets/img/hl-8-re-0.png)

Now, we have done two edge cases, (a) an empty text with an empty pattern and 
(b) a non-empty text with an empty pattern. 

Another edge case, an empty text with a non-empty pattern. This meaning of '\*'
is different from the Wildcard Matching problem that only requires 1 character. 
Here, '\*' requires a preceding character. 

Of course, If **s** is '' and **p** is 'a', this is a mismatch (False). 

If **s** is '' and **p** is 'a\*', 'a\*' matches 0 characters in the text, so
its answer depends on the previous pattern without 'a*'. The previous answer is
T[0][0] (True). 

![](/assets/img/hl-8-re-1.png)

If there is a match for T[i][j], e.g., s[i] is 'a' and p[j] is 'a' or '.', 
T[i][j] depends on its previous answer T[i-1][j-1]. 

![](/assets/img/hl-8-re-2.png)

If p[j] is '\*', there two cases of its preceding character: 

* p[j-1] == s[i]: It can match 0 or 1 characters in the text. If matching 0
characters, then the previous answer is T[i][j-2]. If matching 1 character, 
then the previous answer is T[i-1][j]. 
* p[j-1] != s[i]: It must match 0 characters in the text, T[i][j-2]. 

![](/assets/img/hl-8-re-3.png)

The code is as follows: 

~~~python
class Solution(object):
    
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        s = '$' + s
        p = '$' + p
        n, m = len(s), len(p)
        
        table = [[False]*(m) for _ in range(n)]
        
        table[0][0] = True
        
        # empty string to non-empty pattern
        for i in range(2, m):
            table[0][i] = table[0][i-2] and p[i] == '*'
        
        for i in range(1, n):
            for j in range(1, m):
                
                if p[j] == '.' or s[i] == p[j]:
                    table[i][j] = table[i-1][j-1]
                elif j > 1 and p[j] == '*':
                    if p[j-1] == s[i] or p[j-1] == '.':
                        # table[i][j-2]: match 0 chars in t 
                        #                (skip last 2 chars in the pattern)
                        # table[i-1][j]: match 1 char in t
                        table[i][j] = table[i][j-2] or table[i-1][j]
                    else:
                        table[i][j] = table[i][j-2]
        
        # print table
        return table[n-1][m-1]
~~~
