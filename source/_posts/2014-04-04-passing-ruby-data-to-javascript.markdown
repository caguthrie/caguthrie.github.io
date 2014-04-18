---
layout: post
title: "Passing Ruby Data to Javascript"
date: 2014-04-04 15:36:42 -0400
comments: true
categories: 
---
Using more front-end APIs and frameworks, I've found it useful to be able to, at times, pass data from ruby to my
Javascript files.  The first time I ran into this problem was in <a href="http://avoid.caguthrie.com">Avoid</a>, the
NYC restaurant and health violations lookup tool I helped create.  In this app, I used the Google Maps API to
dynamically update the geographic coordinates of restaurants in bad shape on a google map.  I found this diffucult to
do as you can't normally access Ruby data from Javascript.  I've since found out a few ways to do so, and I'll tell you
about them now.
<br/>
<strong>Data Attributes</strong>
<br/>
One of the simplest ways to pass a very limited amount of data is to use a data attribute on an HTML tag.  For example, 
if I wanted to pass a longitude and latitude from my rails app to Javascript by extracting it through the DOM with
jQuery:
<br/>
```html index.html.erb
<span class="latlong" data-latitude="32.796283" data-longitude="-117.129574">Something that doesn't matter</span>
```
```javascript coords.js
$(".latlong").data("latitude"); //= "32.796283"
$(".latlong").data("longitude"); //= "-117.129574"
```
As you can see, this is realatively simple and straight forward.  It doesn't matter what is inside whatever
tag you have used to store your data, you're just referencing the attributes of the tag itself.  Also, it could be
any type of tag.  This is easy when the data you need to access isn't an object, but it a more primative data type, 
or string.
<br/>
<strong>Gon Gem</strong>
<br/>
The <a href="https://github.com/gazay/gon">Gon Gem</a> is another way to do this, and it is generally more useful
when you are trying to pass more complex data-types, such as objects.  Very simply, just install Gon:
```ruby Gemfile.rb
gem 'gon' # Put this in your Gemfile
```
Then, in your conroller, or really wherever in your app you want to store your data for Javascript use:
```ruby Users_Controller.rb
gon.last_three_users = User.order(name: :desc).limit(3)
```
```javascript users.js
console.log("Welcome our three newest users!");
for( var i=0; i<gon.last_three_users.length; i++ ){
  console.log(gon.last_three_users[i].name + "!");
}
```
Simple enough.  That saves a lot of time and cleans up your views nicely.  No more Javascript hanging out in .html.erb
files.

