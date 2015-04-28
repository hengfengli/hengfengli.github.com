---
layout: post
title: "My explanation to a simple recursive exercise"
categories: [programming]
tags: [recursion]
published: true
---

I am doing the lab demonstrator this semester again for a Python subject. I received an email from a student to ask an exercise about how to reverse a simple list (not nested list) in a recursive way instead of using an imperative method (e.g., for and while loops). My answer is as following: 

For this exercise, first you need to know that a recursion program needs two 
conditions: 1) stop condition; 2) recursive condition (reducing the problem to 
a subset). 

1) **Stop condition.** When should the program stop? If the input is an empty list, 
we should stop the search and return an empty list. Also, if the input is a list
with a single element, we can just return that list because we do not need to 
reverse it. 

{% highlight python %}
if len(string_List) == 0 or len(string_List) == 1:
    return 0
{% endhighlight %}

You checking conditions are correct, but you only need one of them, either an 
empty list or a list with a single element. For your above code, can the 
program reach the first condition (an empty list)? 

the correct code should be the following: 

{% highlight python %}
if len(string_List) == 0:
	return []
{% endhighlight %}

or 

{% highlight python %}
if len(string_List) == 1:
	return string_List
{% endhighlight %}

2) **Recursive condition.**

Let’s assume that you have a list “l” and a function “reverse": 

{% highlight python %}
l = [“a”, “b”, “c”, “d”, “e”]

reverse(l) = reverse(l[1:]) + l[0]
reverse(l[1:]) = reverse(l[2:]) + l[1]
reverse(l[2:]) = reverse(l[3:]) + l[2]
reverse(l[3:]) = reverse(l[4:]) + l[3]
reverse(l[4:]) = reverse(l[5:]) + l[4]
# The recursion stops when meeting your stop condition. 
reverse(l[5:]) = []
{% endhighlight %}

Then, you see when the program puts “reverse(l[5:])” back into previous 
equation, and so on: 

{% highlight python %}
reverse(l[4:]) = [] + l[4]
reverse(l[3:]) = [] + l[4] + l[3]
reverse(l[2:]) = [] + l[4] + l[3] + l[2]
reverse(l[1:]) = [] + l[4] + l[3] + l[2] + l[1]
reverse(l) = [] + l[4] + l[3] + l[2] + l[1] + l[0]
{% endhighlight %}

The above code can be explained that "Reversing a list" equals to “putting the 
head element to the last position and reversing the remaining elements in that 
list”. 

So the entire code should be: 

{% highlight python %}
def reverseLines(string_List):
	# you can use a “print” to check the input for each call 
    # of this function print string_List
	if len(string_List) == 0:
		return []
	else:
		return reverseLines(string_List[1:]) + [string_List[0]]
{% endhighlight %}

Think a bit why we use “… + [string_List[0]]” and why we need to add square 
brackets to "string_List[0]". Hint: the data type of returning value from the 
function “reverseLines()”. 

Writing a recursion function is so simple and elegant, but it is hard to think. 
You have to change a way to think about the problem. Instead of thinking “how” 
to do it, you should think about “what” it should be. 