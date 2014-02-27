---
layout: post
title: "Smooth as Sinatra"
date: 2014-02-27 11:10:24 -0500
comments: true
categories: 
---
<p><em>The best revenge is massive success.<br>-Frank Sinatra</><br><br>Even in death, Sinatra is continuing to make people swoon.  Sinatra is a web framework for Ruby applications and it makes creating and deploying them super simple.  As a Sinatra beginner, I have been looking into the documentation to find some neat tricks with this framework and I'd like to share them with you.  So come fly with me, let's fly away ...</p><br><b><h3>Sinatra Splat</b></h3><br><p>Sintra is flexible in that there are many ways to implement wildcards in pathnames.  For example:</p>
```ruby
get '/favorite_singer/*/favorite_album/*' do
  # example: /favorite_band/frank_sinatra/favorite_album/come_fly_with_me
  params[:splat] # => ["frank_sinatra", "come_fly_with_me"]
end
```
<p>Here, you can let your user go to any sub-path where your wildcards are and get those sub-path names back to you in a handy array using the :splat symbol key in the params hash.  This could be useful in a product catalog, or in any case where there could be a large number of keys that could return data from a database.  There are other useful ways to use splats in pathnames, like for downloads:</p>
```ruby
get '/download/*.*' do
  # goes to /download/path/file.zip
  params[:splat] # => ["path/file", "zip"]
end
```
<p>If you wanted a custom page for your users to look at while they download a file, you could easily do something similar to that.  Sites like cnet.com and download.com may use Sinatra as they similar downloader pages.</p><br><b><h3>Multiple gets for a sub-path</b></h3><br><p>Sometimes you want to have a specific sub-path case, with a catch-all, psuedo-else get method if a different sub-path is chosen.  Looking back at unnecessary complexity of the previous sentence, that may make zero sense to some people.  So, I have an easy example below:</p>
```ruby
get '/the_capital_of/california/:cap' do
  pass if params[:cap].downcase != "sacramento"
  "Correct!!"
end

get '/the_capital_of/california/*' do
  "Your knowledge of 3rd grade US geography is disasterous!"
end
```
<p>Using the pass method in sinatra, the rest of the logic in the first get block is not executed and the next get method fitting the path parameters is executed.  It's another added bonus of the lazy ruby interpreter.</p><br><b><h3>Browser Redirect</b></h3><br><p>Lastly, you may want to redirect a user to a different path after you've taken some action.  Maybe, for example, after you've filled out a form or shown a quick advertisment.  The redirect method is a quick way to do that, and you can implement it like so:


```ruby
get '/restricted_area' do
  "Why are you here!? Go away!"
  sleep 2
  redirect to('/public_area')
end

get '/public_area' do
  "Welcome!"
end
```

So pull up a fine leather chair, a glass of scotch, and enjoy the classiness that Sinatra has to offer you.  If there's a problem in a web app you need solving, old blue eyes has you covered.




