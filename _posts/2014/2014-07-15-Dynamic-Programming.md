---
layout: post
title: Dynamic Programming
categories:
- Programming
tags:
- Algorithm
- Python

---
* Fib Problem
* Knapsack Problem

---

## Fib Problem
According to the definition of fib, we can write these code to calculate fib(n), but in fact, when using the code, we may find it cannot even calculate fib(40).

{% highlight python %}
def fib(n):
    #print "Call fib(", n, ")"
    global nCall
    nCall += 1
    if n == 0 or n == 1:
        return 1
    else:
        return fib(n-1)+fib(n-2)
nCall = 0
print fib(20)
print nCall
{% endhighlight python %}

We find that we call same fib(n) serveral times. To accelate the program, we use the dynamic programming (dynamic here due to historical reasons) skill. We sacrifice space to store result and then return these result directly in the code.

{% highlight python %}
def fastFib(n, m):
    #print "Call fib(", n, ")"
    global nCall
    nCall += 1
    if n == 1 or n == 0:
        m[n] = 1
        return m[n]
    try: return m[n]
    except KeyError:
        m[n] = fastFib(n-1, m)+fastFib(n-2,m)
        return m[n]
def Fib(n):
    m = {}
    return fastFib(n,m)
nCall = 0
print Fib(100)
print nCall
{% endhighlight python %}
In python, I use dict type m to store the result and by using c or c++, we may use array m and  initialize the m to be a zero vector which can never be the fib result.

{% highlight python %}
def fastFib(n, m):
    #print "Call fib(", n, ")"
    global nCall
    nCall += 1
    if n == 1 or n == 0:
        m[n] = 1
        return m[n]
    if m[n] > 0:
        return m[n]
    else:
        m[n] = fastFib(n-1, m)+fastFib(n-2,m)
        return m[n]
import numpy as np
def Fib1(n):
    m = np.zeros(100)
    return fastFib(n, m)
nCall = 0
print Fib1(10)
print nCall
{% endhighlight python %}

## Knapsack Problem
The method that store result and use it directly can be used in the classic Knapsack problem. Image this, you are a burglar and want to stole things but you can only bring 9 pounds and there are 4 ponds gold, a china which is 5 ponds and  a valuable painting in front of you. Burglar need to make a choice to maximize the value that he stole. If we bruteforce the question, it has exponential complexity. we can transform it to be a pesudo poly complexity problem by using dynamic programming.

{% highlight python %}
w = [3, 3, 3, 5,2, 6, 7, 1, 4]
v = [4, 4, 4, 10,2, 12, 8,3, 7]
maxW = 17
def bruteForce(w, v, maxW, i):
    global nCall
    global result
    global result1
    nCall += 1
    if i == 0:
        if w[i] > maxW:
            return 0
        else:
            return v[0]
    else:
        withoutI = bruteForce(w, v, maxW, i-1)
        if w[i] < maxW:
            withI = bruteForce(w, v, maxW-w[i], i-1)+v[i]
            bigger = max(withoutI, withI)
            return bigger
        else:
            return withoutI
nCall = 0
print bruteForce(w, v, maxW, len(w)-1)
print nCall
{% endhighlight python %}

We donot store any intermediate result.

{% highlight python %}
def clever(w, v, maxW, i, m):
    global nCall
    nCall += 1
    try: return m[(maxW, i)]
    except KeyError:
        if i == 0:
            if w[i] > maxW:
                m[(maxW, i)] = 0
                return 0
            else:
                m[(maxW,i)] = v[i]
                return m[(maxW, i)]
        else:
            withoutI = clever(w, v, maxW, i-1, m)
            if w[i] <= maxW:
                withI = clever(w, v, maxW-w[i], i-1, m) + v[i]
                bigger = max(withI, withoutI)
                m[(maxW, i)] = bigger
                return m[(maxW, i)]
            else:
                m[(maxW, i)] = withoutI
                return m[(maxW, i)]
            
def knapsack(w, v, maxW, i):
    m = {}
    res = clever(w, v, maxW, i, m)
    return res, m
nCall = 0
res, m =  knapsack(w, v, maxW, len(w)-1)
print res
print nCall
i = len(w)-1
result = []
while res > 0:
    current = m[(maxW, i)]
    next = m[(maxW, i-1)]
    if current != next:
        result.append(i)
        maxW -= w[i]
        res -= v[i]
        print res
    i -= 1
{%endhighlight python %}
To find the path to the maximize, we need to consider the m. First m[(27, 8)] do not equal to m[(27, 7)] **consider the meaning of dict m**. This means that we should include the 8+1 th item in the package. Second m[(27-4, 7)] does not equal to m[(27-4, 6)] which means that the 7+1th item should also be included, until the whole value, which is the value of res in the problem, equal to zero.
