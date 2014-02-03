---
layout: page
title: About
---

## About me

<div class="vcard">
Hi, I am <span class="fn"><span class="given-name">Sylvain</span> <span class="family-name">Emery</span></span>.
<br />
Find me on <a class="url" href="https://plus.google.com/+SylvainEmery">Google+</a> or on <a href="https://github.com/sylvainemery/">GitHub</a>.
</div>

## About this site

Here is what I use to build this site:

- [Jekyll](http://jekyllrb.com/), a static site generator. I was attracted by the fact that thanks to Jekyll, this site is nothing more than pure HTML/CSS/JS. No database. This is as scalable as it gets.
- [Poole](http://getpoole.com/): foundations and sugar on top of Jekyll.
- The [Hyde theme](http://hyde.getpoole.com/) for Poole. Clean, although I modified its `h1` style a bit.
- [Google+ Comments](https://support.google.com/blogger/answer/2981015). Because this site has no DB, I can't host the comments, so I have to use an external service. I chose Google+ because this is where I spend most of my "social" time. And even though there is [no official plugin](http://googlesystem.blogspot.fr/2013/04/add-google-comments-to-any-web-page.html) yet, I just love how easy it is to put in on a site.
- This blog lives in [GitHub Pages](http://pages.github.com/). This is free and very easy to use with Jekyll - as simple as a `git push`. It also means I don't have to manage a server.
- Because I mainly work on Windows laptops (for now), I use [Vagrant](http://www.vagrantup.com/) to start an Ubuntu 12.O4 VM. Jekyll runs here. Hacking on Linux is always easier!
- Vagrant enables me to mount a host-located directory in the VM. This way, I can use [Sublime Text](http://www.sublimetext.com/) in Windows to edit my files, and Jekyll serves it from the VM. Unfortunately, I couldn't make the watch option of Jekyll work in this configuration. So no live reload for me...

Everything is in a [public repo](https://github.com/sylvainemery/sylvainemery.github.io) on github.
