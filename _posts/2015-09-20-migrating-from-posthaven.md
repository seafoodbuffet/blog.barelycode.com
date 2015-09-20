---
layout: post
title: Migrating the blog to Github
tags:
  - posthaven
  - jekyll
  - github
  - DNS
---

After basically a year off (because I'm lazy), I decided to pick this blog back up.
Feeling guilty that I wasted a year's worth of payments to [Posthaven](http://posthaven.com)
and being a little frustrated over the lack of control that Posthaven offered (for example,
I wanted to experiment with [Universal Analytics](https://support.google.com/analytics/answer/2790010?hl=en)
that Posthaven simply doesn't support) I decided to look for new hosting.

I ended up choosing to build the blog with [Jekyll](https://jekyllrb.com) and
hosting on [GitHub Pages](https://pages.github.com). I've written about GitHub Pages
before when I discussed building the
[Mobile Insult Generator]({% post_url 2014-02-10-building-the-mobile-insult-generator %}/).
It turns out that GitHub Pages is a pretty popular way to host blogs as well and Jekyll
makes that process rather easy.

Jekyll is a static site generator. In the rage of dynamic websites, Jekyll lets you
author content in markdown or some easier-than-html language and then generates the
underlying HTML as static content. Because the HTML is static, it's easy to serve
and doesn't drain server resources as much as a traditional CMS. GitHub has
[documentation on using Jekyll with GitHub](https://help.github.com/articles/using-jekyll-with-pages/).
The main thing to be aware of is that you should use the gh-pages gem and that it's
currently using an older version of Jekyll (2.4.0) than many of the Jekyll templates
are built using. This blog for example uses [Lanyon](http://lanyon.getpoole.com)
where the shipped \_config.yml isn't understood by Jekyll 2.4.0

Migration presented a few small issues. Most of these I overcame through brute force.
Posthaven supports export, but only to a Wordpress bundle. Luckily this bundle is
just an RSS feed so it was easy to pick the content out. Had I written a lot more
posts, I probably would have to write a migrator, but because of the small number of
posts, it was way easier to just manually re-write each post as Markdown.

Image hosting turned out to be interesting too. By default, you can upload images
to Posthaven, they turn around and store that in S3. Jekyll can serve image content
also but it lacks lots of nice things like auto image sizing, plus you're now
managing image binaries in a GitHub repo (which I really don't like). For now,
I'm manually resizing images and serving out of GitHub pages, but I may end up migrating
to a different image host like [imgur](http://imgur.com) if this gets old.

Last step is migrating DNS away from the old *blog.barelycode.com* to GitHub. GitHub
pages has this [somewhat hard to find page](https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider/)
that actually documents the IP addresses you'll need to put in order for your custom
domain to be served up by GitHub pages. Last thing is waiting out DNS. For security
reasons, I set all my DNS TTL's to be 1 day after [this rather harsh story](https://medium.com/@N/how-i-lost-my-50-000-twitter-username-24eb09e026dd).
Unfortunately, that means legitimate changes take a whole day to propagate too. What I
_should_ have done is to change the TTL to 15 minutes a day before I actually wanted to make
the change. Then do all the migration, test to make sure the new DNS is up and running and then reset
the TTL back to 24 hours. This kind of waiting right now is infuriating :P
