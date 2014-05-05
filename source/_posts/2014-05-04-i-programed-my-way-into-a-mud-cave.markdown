---
layout: post
title: "I programmed my way into a mud cave"
date: 2014-05-04 16:59:46 -0700
comments: true
categories: 
---
I'm a big fan of Geocaching.  If you are unfamiliar with it, it's a worldwide activity, where by using a GPS you can search out 
hidden containers.  These containers contain a logbook and usually other small toys and trinkets.  There are over a million
geocaches worldwide in over 200 countries and territories.  Learn more about it <a href="http://www.geocaching.com">here</a>.

This post is about geocaching, programming, and the surprise I found when I was able to make them intersect.  A special "puzzle" 
geocache that I came across required you to unscramble <a href="http://imgcdn.geocaching.com/cache/large/5fde6914-cd6d-4869-b4b9-57ccb20bdfde.jpg">this encrypted message</a> using <a href="http://imgcdn.geocaching.com/cache/large/492970f4-9a6b-40df-a038-610204ff92f5.jpg">this fuzzy picture</a> and the coordinates N32° 40.990 W117° 11.025 as hints.  I knew the real location for the cache was somewhere in the deserts of San Diego county, and I had to solve the puzzle to find the coordinates.  

{% img left https://farm8.staticflickr.com/7317/14108703315_8dee144405_z.jpg 320 250 'Down into the abyss' %}

I had figured out the methodology to the decryption and that another word was required as a key.  From that, I knew that once I 
had found the key, the manual process of decrypting that whole message could take hours.  Enter coding.  I wrote a program that 
accepted any key and decoded the message with the inputted key.  Not yet knowing what the key was, I attempted to cheat and 
iterate through the entire dictionary to see if anything comprehensible was returned.  That failed, and my corner cutting 
mission was thwarted.  Therefore, I knew that the key was not a normal dictionary word.  

You can find the program on Github <a href="https://github.com/caguthrie/tombraider_geocache">here</a>.

{% img right https://farm6.staticflickr.com/5502/13922089900_eaec5db590_z.jpg 320 250 'In the cave' %}

I don't want to give anything away, but I eventually figured it out.  Anyone can figure it out, it isn't necessary to go to 
those coordinates, so give it a shot and run it through my program if you think you've got it.  

The program was relatively simple, and all I did was extend the String class by adding a method that returned the ASCII value 
of the character.  This made the rest of the program easy, as it was just arithmetic from there on out.  

```ruby
class String
  def to_num
    self.downcase.bytes[0]
  end  
end
```
{% img left https://farm3.staticflickr.com/2936/14109093254_9847367727_z.jpg 250 320 'Found it!' %}

Once the puzzle was solved, the trek to the Arroyo Tapiado mud caves in the Anza-Borrego desert was a one filled with 
anticipation.  After a grueling hike through the low ceilings and narrow passageways of this mud cave, we emerged, climbed up a 
few more small mountains, and located the cache.  An amazing adventure all around.  An adventure with a big assist from code.

If anyone knows of any other caches what were solved with code, or knows of any amazingly adventurous caches such as this, 
please let me know.  I'd love to do them.

{% img http://i.minus.com/i6oC7Id9ijSe1.JPG 890 280 Anza Borrego %}