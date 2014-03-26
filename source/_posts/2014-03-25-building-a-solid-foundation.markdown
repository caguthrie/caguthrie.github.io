---
layout: post
title: "Building a Solid Foundation"
date: 2014-03-25 22:18:56 -0400
comments: true
categories: 
---
I'm a math guy, a numbers dude, and and analytical person at my core.  The one thing about web design that scared 
me the most is the front-end.  Have you seen my artistic side?  One time I drew a stick figure and the paper exploded.
True story.  Thankfully, things like Twitter Bootstrap and Foundation exist for artistically inept people such as 
myself to look like we majored in a liberal art.

For those who don't know, Foundation is basically just a huge CSS library that makes your website actually look
visually appealing.  You can add class attributes to your HTML elements for control over your layout and other
basic functions.  For a rails app, here is the long and tedious process to install Foundation:

```ruby
gem 'foundation-rails' # Add this to your Gemfile
```

Then, on your command line, run 'bundle install' and 'rails g foundation:install' to generate the Javascript
and CSS files it needs.  You're done!  Phew!

Foundation will add a few files to your /assets/javascript and /assets/stylesheets directories.  The main one
you should care about is the foundation_and_overrides file in the stylesheets directory.

The main thing you should know when styling your page with Foundation, is that each "row" is split up into 12 the width of 12 columns.  All the neat features are in classes you add as attributes to your tags.  For example:

```html
<div class="row">
  <div class="small-4 columns">
    This is some text on the first third of the page, vertically.
  </div>
  <div class="small-8 columns">
    This is some text on the latter two-thirds of the page, vertically.
    <a href="http://www.google.com" class="button">This button goes to Google</a>
  </div>
</div>
```

As you can see, the number of columns in a row add up to 12, as should be.  You can also nest rows and columns 
inside other rows and columns.  For a stylistically challenged person such as myself, this simplicity is 
invaluable.  

But let's say you want to style your links so that text link fonts are red.  Remember that foundation_and_overrides
file I talked about earlier?  Go to that file and look for anchor tag styling ... ah, here it is!

```javascript
// We use these to style anchors
// $anchor-text-decoration: none;
// $anchor-font-color: $primary-color;
// $anchor-font-color-hover: scale-color($primary-color, $lightness: -14%);
```

Now all you do is uncomment the font color and replace it with hex red:

```javascript
// We use these to style anchors
// $anchor-text-decoration: none;
$anchor-font-color: #f00;
// $anchor-font-color-hover: scale-color($primary-color, $lightness: -14%);
```

Pretty easy huh.  You can still go and define your own CSS if you want, but this process makes things much
easier.

One thing I was most worried about when getting the front-end all nice and pretty was the navigation bar.  With
foundation, key in a few div tags and you're done.

```html
<div class="contain-to-grid sticky">
  <nav class="top-bar" data-topbar>
    <!-- Left Nav Section -->
    <ul class="left">
      <li><%= link_to "App Name", root_url %></li>
    </ul>
    <!-- Right Nav Section -->
    <ul class="right">
      <li><%= link_to "Products", products_path %></li>
      <li class="has-dropdown">
        User Info
        <ul class="dropdown">
          <li><%= link_to "Sign-In", login_path %></li>
          <li><%= link_to "Sign-Up", new_login_path %></li>
        </ul>
      </li>
    </ul>
  </nav>
</div>
```

The div class sticky makes it so if you scroll down, the nav bar remains fixed to the top of your screen.  I don't
like that, but I thought I'd show you anyway.  Everything else is pretty intuitive, just make sure you add the right
class to your tags and you're good to go.

In all, Foundation is a quick way to make your website actually visually appealing in no time at all.  And if you're
artistically retarded such as myself, it makes you passable for at least starving-artist status.

