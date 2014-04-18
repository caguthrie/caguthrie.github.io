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

Here, we attempt to divide each number by each number smaller than it.  If we are calling this loop on
<script type="math/tex">n</script> elements, then this will loop <script type="math/tex">t</script> times
for every<script type="math/tex">n_t</script>, for a time complexity of <script type="math/tex">O(n^2)</script>.  

As the problem states, we need to compare the sum of the proper divisors to the proper divisors of the sum itself.
Therefore, we call the above method twice in this method:

```ruby
def is_amicable?(num)
  a = find_sum_of_divisors(num)
  b = find_sum_of_divisors(a)
  if num == b && a != b
    true
  else
    false
  end
end
```

So, we wrote this program.  Let's see how long it takes:

```bash
This took 5.733192 seconds
```

Ok, let's try to do better.  First of all, let's think of a math way to fix that find_sum_of_divisors method. No ruby
magic here.  Since there are no integers less than 2 that divide into any number to come up with a proper divisor, all
the numbers <script type="math/tex">a</script> given an <script type="math/tex">n</script> in
<script type="math/tex">n>=a>n/2</script> do not need to be evaluated as those <script type="math/tex">a/n</script> will
always be <script type="math/tex"><2</script>.  So instead of num.times, we can change it to:

```ruby
def find_sum_of_divisors(num)
  # can optimize this by going up to half of num
  sum = 0
  (num/2).times do |i|
    sum += (i+1) if num % (i+1) == 0 && num != (i+1)
  end
  sum
end
```

Now let's try running the program:

```bash
This took 2.88388 seconds
```

We've, predictably, sliced our running time in half.  Great!  But we can do better.  Remember memoization?  Let's do
that.

When we run this problem, it is very possible that we will be running find_sum_of_divisors on the same number multiple
times.  Because this is a posibility, we can try storing the result of each method call in a hash and referencing
it if we need it again, instead of calling the same method again.

To do this, let's initialize an empty hash each time we instantiate a new Amicable object.

```ruby
def initialize(max)
  @max = max
  @hash = {} #<--- Add this line
end
```

Finally, every time we go to call the find_sum_of_divisors method, we should check to see if we already have for a
given number.  If we do, we can just use that data and not call the method, saving valuable time:

```ruby
def is_amicable?(num)
  @hash[num] ||= find_sum_of_divisors(num)
  @hash[@hash[num]] ||= find_sum_of_divisors(@hash[num])
  if num == @hash[@hash[num]] && @hash[num] != @hash[@hash[num]]
    true
  else
    false
  end
end
```

Great, now instead of calling a method, we can refernce data in a hash.  Let's see how much faster we made it:

```bash
This took 1.997799 seconds
```

Just under two seconds, awesome!  I'm sure there's even more optimizations possible.  Submit a pull request to my code
if you want to try.  You can check out my Github repo for the problem <a href="https://github.com/caguthrie/pe21">
here</a>.



