---
layout: post
title: "Memoization and why does it feel so good to optimize?"
date: 2014-04-18 11:14:58 -0400
comments: true
categories: Ruby, Math, Optimization
---
Recently I was faced with a challenge from <a href="http://projecteuler.net/">Project Euler</a> and asked how I would
best optimize the solution I wrote.  Thinking for a little bit, I made a few changes, but none were as interesting as
as term that I had never heard before, but understood the concept: Memoization.

Memoization is the concept of storing a data in memory that you may reference again at a later time, instead of
calling a function with parameters you have already passed it previously.  I'll show you how we can use memoization
and a little bit of logic to optimize a problem from Project Euler.

<strong>Project Euler Problem #21: Amicable Numbers</strong>

```
Let d(n) be defined as the sum of proper divisors of n (numbers less than n which divide evenly into n).
If d(a) = b and d(b) = a, where a ≠ b, then a and b are an amicable pair and each of a and b are called amicable numbers.

For example, the proper divisors of 220 are 1, 2, 4, 5, 10, 11, 20, 22, 44, 55 and 110; therefore d(220) = 284. The proper divisors of 284 are 1, 2, 4, 71 and 142; so d(284) = 220.

Evaluate the sum of all the amicable numbers under 10000.
```

You can find my first solution <a href="https://github.com/caguthrie/pe21/blob/master/amicable.rb">here</a>.  Obviously,
the most expensive operations in an program are those which iterate or recurse.  In this case, we have one different
loop that looks particularly expensive:

```ruby
def find_sum_of_divisors(num)
  # can optimize this by going up to half of num
  sum = 0
  num.times do |i|
    sum += (i+1) if num % (i+1) == 0 && num != (i+1)
  end
  sum
end
```

Here, we attempt to divide each number by each number smaller than it.  If we are calling this loop on n elements, then
this will loop t times for every [latex]n_t[/latex], for a time complexity of \O(n^2).  




