---
layout: post
title: Getting a shortened vanity domain
tags:
  - dns
  - hover
  - bit.ly
  - url_shortening
---

I recently started tweeting again recently. You can follow me [@barelycode](https://twitter.com/barelycode).
Living within Twitter's 140 character limit means that a lot of URLs posted
in tweets have to be shortened, giving rise to a [bunch](http://tinyurl.com) of
[different](http://bitly.com) URL shortening services. Of course, it wouldn't do
at all for a big web presence like Google or even a small web presence like me
to post using some else's shortened name. The Libyan economy must partly be
propped up by all of the registered [.ly domains](http://www.nic.ly). Anyway,
I'm going to talk about how I managed to secure my own vanity shortening domain
[brlyco.de](http://brlyco.de).

First thing you should know is that bitly.com nicely will allow you to use a
custom domain for shortening with their service.
[Instructions on how to do this](http://support.bitly.com/knowledgebase/articles/76741-how-do-i-set-up-a-custom-short-domain).
Since ideally, you're generally looking for the shortest possible domain, one
often turns to exotic top level domains (TLDs) like .de .ly .gl .co and such
instead of .com or .org

So first thing you gotta do is pick a domain. This is where a domain suggester
like [Domai.nr](https://domai.nr) can be really helpful. In my case, I ended
picking a .de domain. A lot of places let you register .de domains, but you
will quickly find that DENIC, Germany's central registry for all .de domains
requires that you have a German address in order to register a .de domain.
Some registrars, most notably [EuropeRegistry](http://www.europeregistry.com)
will offer to list their German location as your contact address. This is akin
to domain privacy where a proxy is listed as the contact information in the
WHOIS database. Unfortunately, EuropeRegistry charges a whopping €19 for
registering a German domain. Being super-cheap, and seeing that I could get the
same thing from Hover for $15, all I needed to do was to get a German address.
Now you could simply make one up, but that's risky. What if someone tries to
contact you? What if you need to prove that you actually own the domain by
having something sent to your address on file? A fake address or an address
not under your control is risky in this case. You don't want to accidentally
lose a domain you worked so hard to build up. Enter mail and package forwarding
services. These services will provide you an honest to goodness German
(or Swiss or whatever country) mailing address. All your mail then gets
forwarded to you. This lets you buy goods only shipping to Germany say and
then you only pay shipping costs to then receive them in the US or wherever.
Best of all, the service I went with, [Mailboxde.com](http://mailboxde.com)
charges nothing unless you actually receive something.

So with a real, valid German address assigned to me, I registered brlyco.de
through Hover (I wanted to try a new registrar after all of the GoDaddy hate
and all of the Hover love. The process was painless and minutes later, my
domain was up and running.

All that was left was to set an A record for the domain pointing at bitly and
for bitly to recognize the change and I was good to go. Pretty easy eh?

Finally, as an aside, despite all of the Hover-love on the internet, I've
actually not been incredibly impressed yet. For example, they require you to
hand over your username/password for your GoDaddy account in order to affect a
migration of domains. Unlike GoDaddy, Hover doesn't support multi-factor
authentication. Worst of all, they have a seemingly unchangeable 15-minute TTL
on domain changes. That's like, super horrible since any bad side effects
(or hacked domain changes) take place immediately. So Hover, I know people love
you, I'm still waiting to figure out why. But I'm a paying customer so we'll see.
