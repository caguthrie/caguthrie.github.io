---
layout: post
title: "Enumberable is Enumermazing"
date: 2014-02-12 13:35:30 -0500
comments: true
categories: Ruby, Enumberable
---
<p>Coming from lower-level languages, the vast spectrum of built in classes, modules, and methods in Ruby is absolutely mind-blowing.  One of the most amazing of these is Enumerable.  Enumerable is a mixin, which means that it adds functionality to the class that uses it.  Oh, and functionality there is.  Enumerable is robust and adds some methods that do exactly what you think they would.  For example:</p>
```ruby
find
any?
include?
```
<p>The functionality I have found most useful are the searching and sorting methods.  I'll tell you about three of my favorites now.</p>

<b><h3>sort</h3></b>

<p>The sort method might seem like a shy, simplistic wallflower, but, with the power of blocks in ruby, sort transforms into a something any high school prom king would tremble at the sight of.  Take this example:</p>
```ruby
favorite_people = {
    :poet => "Walt Whitman",
    :baseball_player => "Tony Gwynn",
    :jeopardy_champion => "Ken Jennings",
    :billionaire => "Bill Gates",
    :dean => "Avi Flombaum"
  }
```
<p>Let's say you have this hash of favorite people and wish to sort it into an array, based on alphabetical order of their last name.  Is that description of what I want to do longer than the code:</p>
```ruby
favorite_people.sort{ |a, b| a[1].split(" ").last <=> b[1].split(" ").last}

#=> [[:dean, "Avi Flombaum"],
#   [:billionaire, "Bill Gates"],
#   [:baseball_player, "Tony Gwynn"],
#   [:jeopardy_champion, "Ken Jennings"],
#   [:poet, "Walt Whitman"]]
```
Yep.

<p>When used on a hash, sort converts your hash to an array of arrays of length 2, where each sub-array is a key/value pair.  This is defined in the hash's each method, a method I will talk more about a little later.  Enumerable's sort function uses these return values to sort your input.  The great thing about the sort method is that you can sort it however you tell your program to.  No more iterating though your entire data structure.  This is even more relevant because if you have an object you would like to use with Enumerable, all you have to do is define an each method in your class, which yields each value you would like Enumerable to operate on.</p>
```ruby
class DeliciousFood

  include Enumerable

  def yum
    ["Tangerine", "Burrito", "Cake", "Artisanal Pizza", "Bacon Grease"]
  end

  def each
    yum.each do |y|
      yield y
    end
  end
end

DeliciousFood.new.sort { |a, b| a <=> b } #=> ["Artisanal Pizza", "Bacon Grease", "Burrito", "Cake", "Tangerine"]
```
<p>You want your class to be Enumermazing?  Just like that, it's as easy as defining an each method.</p>

<b><h3>grep</h3></b>

<p>The method grep is, just as you command line monkeys would think, a function that allows you to search through your data and find what matches your search parameter(s).  Just like sort, and many other methods in Enumerable, grep can take a block to create your own custom searches.  Let's return to our DeliciousFood class above and try searching for each piece data that includes the letter "k" or "z", because I only eat food that contains at least one of those letters:</p>
```ruby
DeliciousFood.new.grep(/(k|z)/) { |a| "I ate some " + a } #=> ["I ate some Cake", "I ate some Artisanal Pizza"]
```
<b><h3>partition</h3></b>

<p>Lastly, a fun, easy, and quick way to put things into two different arrays.  The partition method takes in some data, and outputs it in an array of two arrays.  If the block passed into it evaluates as true, it gets placed in the first sub-array.  If not, it gets placed in the second sub-array.  Lets say I'm hungry but I'm confused about what to eat from my kitchen table:</p>
```ruby
food = ["Tangerine", "Burrito", "Cake", "Artisanal Pizza", "Bacon Grease"]
not_food = ["Concrete", "Fedora", "Silica Gel", "Cigarette Butt"]

my_kitchen_table = ["Concrete", "Tangerine", "Burrito", "Fedora", "Cake"]

output = my_kitchen_table.partition { |item| food.include?(item) } #=> [["Tangerine", "Burrito", "Cake"],["Concrete","Fedora"]]
puts output[0] #=> ["Tangerine", "Burrito", "Cake"]
```
<p>Thankfully Enumerable and partition saved me from eating concrete or my fedora.  What can't Ruby do?</p>