---
layout: post
title: Building the mobile insult generator
tags:
  - mobile
  - iOS
  - development
  - javascript
  - bootstrap
---

I had done it, one the glorious screen were the words *ugly turd pirate*.
Maybe not the most wonderful thing anybody's ever built, but the victory was
mine anyway. I find myself having less and less time to code stuff, thus this
was a particularly satisfying result as I managed to crank it out in about two
hours flat. Earlier in the day, I saw [this post](http://www.reddit.com/r/funny/comments/1xa0k9/ill_be_ready_the_next_time_i_get_called/)
on reddit and felt myself inspired to build an app around this. Knowing that I
wanted to make this a mobile app, I figured on building it using responsive
bootstrap and leave it at that. Without further ado, here's the awesome mobile
insult generator.

![insult generator]({{ site.url }}/assets/mobile-insult-screenshot.png)

Okay, so maybe not the most complicated thing in the world, but there was a
number of interesting things that went into this. First of all, it's built as
an entirely static web page, there's nothing server-side at all other than to
serve the content. The entire "trick" is to have the insult parts in
javascript, which is randomly selected client-side. To do this, I got to play
with [underscore.js](http://underscorejs.org) for the first time. The
*_.sample* function provided exactly the behavior I needed (randomly sample a
single element from the array. As the rest of the world has already
discovered, [underscore is cool](http://www.joelhooks.com/blog/2014/02/06/stop-writing-for-loops-start-using-underscorejs/).
As such, the code to pull this off was simply this: 
{% highlight javascript %}
first:  ['lazy', 'stupid', 'insecure', 'idiotic', 'slimy', 'slutty',
		'smelly', 'pompous', 'communist', 'dicknosed', 'pie-eating', 'racist',
		'elitist', 'white trash', 'drug-loving', 'butterfaced', 'tone deaf',
		'ugly', 'creepy'],

second: ['douche', 'ass', 'turd', 'rectum', 'butt', 'cock', 'shit', 'crotch',
		'bitch', 'prick', 'slut', 'taint', 'fuck', 'dick', 'boner', 'shart', 'nut', 'sphincter'],

third: ['pilot', 'canoe', 'captain', 'pirate', 'hammer', 'knob', 'box', 'jockey',
		'nazi', 'waffle', 'goblin', 'blossum',  'biscuit', 'clown', 'socket', 'monster',
		'hound', 'dragon', 'balloon'],

insult: function() {
	return _.sample(this.first) + ' ' + _.sample(this.second) + ' ' + _.sample(this.third);
}
{% endhighlight %}

Secondly, I learned how to host a github project page off of a custom domain.
The documentation wasn't exactly easy to follow. The bottom-line is that you
need a CNAME file in your gh-pages branch (btw, it MUST be named gh-pages,
no exceptions). In my case, the CNAME was *insult.barelycode.com*. With that
done, you just setup A records in your DNS pointing to the two IPs in the
[custom DNS documentation](https://help.github.com/articles/setting-up-a-custom-domain-with-pages)
I wonder what happens if those IP addresses change?

Finally, adding the "Add to Home screen" turned out to be a lot easier than
I expected. There's [already a library](https://github.com/cubiq/add-to-homescreen)
to do this. The only "trick" is to create a link of type apple-touch-icon in
your html. And make sure that it's big enough (over 100px square) so that the
icon looks nice even on retina iPhone.

All in all, a reasonable effort. I showed this to the wife, whose only comment
was, "wow, how about you build something actually useful?"

*Small victories, small victories.*
