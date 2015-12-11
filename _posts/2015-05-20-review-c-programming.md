---
layout: post
title: "Review C Programming"
categories: [interview]
tags: [interview]
published: false
---

After the MS interview, I realize that I need to review my C
programming because I don't know C any more (the last time 
that I wrote a C program was 4 years ago). It is really stupid 
that I put C into one of my proficient skills. I should practice 
my C programming more just for the interview purpose. 

My plan is to read my first programming book - "the C programming 
language" - again. But this time it would be really fast (and I won't
cover the chapter 8, because it is out of my purpose). I will 
record some important notes here. 

**Table of Contents**

*   [Chapter 1 - A Tutorial Introduction](#ch01)

### <a name="ch01"></a>Chapter 1 - A Tutorial Introduction

#### 1.1 Getting Started

Standard IO library: 

{% highlight cpp %}
#include <stdio.h>

int main()
{
    printf("hello, world\n");
}
{% endhighlight %}

#### 1.2 Variables and Arithmetic Expressions

Basic data types: 

| Data Type     | Bytes         |
| ------------- |:-------------:|
| int           | 4 bytes       |
| float         | 4 bytes       |
| char          | 1 bytes       |
| short         | 2 bytes       |
| long          | 8 bytes       |
| double        | 8 bytes       |

Some space cost models: 

| Data Type      | Bytes         |
| ---------------|:-------------:|
| int[10]        | 40 bytes      |
| char[]="hello" |  6 bytes      |


Formatted print: 

| Format        | Meaning                                         |
| ------------- |:-----------------------------------------------:|
| %d            | decimal integer                                 |
| %6d           | decimal integer, at least 6 characters wide     |
| %f            | floating point                                  |
| %6f           | floating point, at least 6 characters wide      |
| %.2f          | 2 characters after the decimal point            |
| %6.2f         | at least 6 wide and 2 after decimal point       |


#### 1.3 The For Statement


{% highlight cpp %}
for (fahr = 0; fahr <= 300; fahr = fahr + 20)
    printf("%3d %6.1f\n", fahr, (5.0/9.0)*(fahr-32));
{% endhighlight %}


#### 1.4 Symbolic Constants

{% highlight cpp %}
#define    LOWER  0
#define    UPPER  300
#define    STEP   20
{% endhighlight %}

#### 1.5 Character Input and Output

{% highlight cpp %}
// c = getchar()  =>  read a char
// putchar(c)     =>  print a char

#include <stdio.h>

int main()
{
    int c;

    while ((c = getchar()) != EOF)
        putchar(c);
}
{% endhighlight %}

#### 1.6 Arrays

Define an array with a fixed size, and remember to initialize it: 

{% highlight cpp %}
int ndigit[10];

for (i = 0; i < 10; i++)
    ndigit[i] = 0;

{% endhighlight %}


#### 1.7 Functions

A function definition has this form: 

{% highlight text %}
return-type function-name(parameter declarations, if any)
{
    declarations
    statements
}
{% endhighlight %}


#### 1.8 Arguments -- Call by Value

**IMPORTANT:** 
In C, all function arguments are passed "by value". 

This means that the called function is given the values 
of its arguments in **temporary variables** rather than 
the originals. 

The function argument is **"a local copy"**. 

The main distinction is that in C the called function cannot 
directly alter a variable in the calling function; it can only 
alter **its private, temporary copy**. 

The caller must provide the **address** of the variable to be 
set (technically a **pointer** to the variable), and the called
function must declare the parameter to be a pointer and access
the variable indirectly through it. 

The story is different for **arrays**. When the name of an array
is used as an argument, the value passed to the function is 
**the location or address of the beginning of the array** -- 
there is no copying of array elements. By subscripting this value, 
the function can access and alter any element of the array. 


#### 1.9 Character Arrays

{% highlight cpp %}
# define MAXLINE 1000
char line[MAXLINE];
char longest[MAXLINE];

/* copy: copy 'from' into 'to'; assume to is big enough */
void copy(char from[], char to[])
{
    int i;
    
    i = 0;
    while ((to[i] = from[i]) != '\0')
        ++i;
}
{% endhighlight %}

**REMEMBER:** '\0' is the null character, whose value is zero. 

{% highlight cpp %}
"h|e|l|l|o|\n|\0"
{% endhighlight %}


#### 1.10 External Variables and Scope

The keyword **"extern"** is to declare a global variable in a function: 

{% highlight cpp %}
int max;
char longest[MAXLINE];

int main()
{
    extern int max;
    extern char longest;
}
{% endhighlight %}


