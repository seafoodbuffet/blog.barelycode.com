---
layout: post
title: Recovering from a major Boo Boo
tags:
  - iOS
  - ruby
  - security
  - SHA1
---

Over the weekend, while trying to SIM unlock a phone, I realized that the phone had a
[restrictions passcode](https://support.apple.com/en-us/HT201304) set. If you're not
familiar, it's basically *another* passcode that enables parental controls. This
allows you to hand your kids an iPhone while still asserting some level of control.
The problem comes when this infrequently-used and easily-forgotten passcode is
actually forgotten. Annoyingly, as you have repeated failed attempts to guess the
passcode, the wait time becomes 60 minutes between retries which makes
brute force impractical. The forums are full of posts with people's woes. Here's
a breakdown of the issue:

* Unlike the iPhone's normal passcode, the restrictions passcode is preserved during
a backup/restore. Thus you can only get rid of it by wiping the phone. Methods suggesting
a phone wipe are trying to get rid of the iPhone **lock** passcode. The restrictions
passcode cannot be gotten rid of via the wipe and restore method.
* There are [commerical software](http://apple.stackexchange.com/questions/55528/how-do-i-remove-the-restriction-passcode-on-my-ipad)
that help you do this by basically hacking your backup. You backup your phone, edit your backup
and then restore and it will allow you to override your restrictions passcode. This method
probably works fine.
* There's also commercial software that will examine your backup and extract the restrictions
passcode, but they usually reserve that as a paid feature. If you're desperately locked out,
this is an entirely legitimate way to go.

### Find the stubborn way
Being rather stubborn, and figuring that if commercial software could do this, surely I could
do it too, I researched online and found a few things.
iOS backups obfuscate the file names by hashing the names into hashed names.
However, the contents aren't encrypted unless you make an encrypted backup.
It turns out that the restrictions passcode is stored as an HMAC SHA1 hash right
next to the salt. It uses the [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) to generate
this hash. There's a [nice post](https://nbalkota.wordpress.com/2014/04/05/recover-your-forgotten-ios-7-restrictions-pin-code/)
that details all of this. The bottom line is that if you can find the file where your
hashed passcode/salt pair are stored, and if you can repeat their process, there's only 10000
(0000 - 9999) possible four-digit passcodes which makes brute-forcing this totally practical.
In fact, I'm certain that's what the commerical software packages are doing.

### Implementing PBKDF2
It turns out that there's already a nice, [online version](http://1024kb.co.nz/ios-78-passcode-hack/)
of this process. You just need to find the hash/salt and plug them into a website and it
does the rest. Not being satisfied to leave well-enough alone, and because the website
was sorta, kinda slow, I wanted to implement a faster version.

### Enter pbkdf2-ruby
I could have picked whatever language but since I've been working on Ruby at work,
I decided to try that here. It turns out that there's a nice
[Ruby library](https://github.com/emerose/pbkdf2-ruby) that implements pbkdf2.
So I loaded it up, installed the gem, wrote some code (which looks like the below)

{% highlight ruby %}
PBKDF2.new do |p|
  p.password = "s33krit"
  p.salt = "nacl"
  p.iterations = 10000
end
{% endhighlight %}

Immediately, I got this error:

    /gems/ruby-2.0.0-p643/gems/pbkdf2-0.1.0/lib/pbkdf2.rb:147:in `^': nil can't be coerced into Fixnum (TypeError)

WTF? It's like literally the sample code. So it turns out, it's because the version of the
Gem that works is *v0.2.0* whereas the version that's in Rubygems is *v0.1.0*. The Gem author
hasn't seemed to care or bothered to publish the latest to Rubygems. Oops.

However, it turns out that it **is** possible to get a Gem directly from Git/GitHub
using [bundler](http://bundler.io). You just need a Gemfile that looks like this:

    source 'https://rubygems.org'
    gem 'pbkdf2', :git => 'git://github.com/emerose/pbkdf2-ruby'

There's lots of other options include tags/branches, but for me, this was sufficient.
The only last trick is that you'll need to require *bundler/setup* in your file
because the version from Git doesn't show up in your locally-installed Gems.

And gloriously, with that in place, it only took a few lines of code to get a passcode
extractor that works perfectly.

### The code
If you're interested, all the code and more gory details about how to use it are in
my repo: [https://github.com/seafoodbuffet/iOSRestrictionsExtractor](https://github.com/seafoodbuffet/iOSRestrictionsExtractor)

Enjoy and hope this helps if you ever run into this sort of thing.
