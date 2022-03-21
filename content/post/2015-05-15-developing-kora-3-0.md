---
author: Greenjam94
categories:
- Uncategorized
date: "2015-05-15T13:24:11Z"
guid: https://hackerunder.dev/?p=89
id: 89
tags:
- blog.greenjam94.me import
title: Developing KORA 3.0
url: /developing-kora-3-0/
xyz_smap:
- "1"
---

This is a big project I’ve worked on from the beginning when working at [Matrix: Center for Digital Humanities &amp; Social Sciences](http://www2.matrix.msu.edu/). It’s taking an old platform and revamps it into a modern application. KORA 1.0 was built over the last two decades by non-software developers, I never saw the code personally but I heard horror stories of unorganized pages of code that was thousands of lines long.

KORA 2.0 reorganized the code into an [Object-Oriented-Programming](http://en.wikipedia.org/wiki/Object-oriented_programming) (OOP) format, Matrix’s system admin (now retired) and students introduced classes and actually made the code readable to developers. The current release of [KORA (2.6.3)](http://sourceforge.net/projects/kora/?source=directory) is the latest version we have released to the public.

If you’re still asking, what the heck is KORA anyways? Well, it’s a african instrument.

<iframe allowfullscreen="" frameborder="0" height="375" loading="lazy" src="https://www.youtube.com/embed/wneaapj9as8?feature=oembed" width="500"></iframe>

But the application is a way to store analog data (Pictures, books, interviews, etc) into a digital database. Picture if you will, a way for non-developers to create, update, and use a database without even realizing it.

KORA 3.0 is using a PHP framework called [Laravel](http://laravel.com/) to further improve the code layout. We’re introducing a [MVC](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) layout to separate content pages, objects and actions, and controllers. The plan is to also restructure the database tables and format all the classes to match a theme to make it easier for new students to continue developing it. KORA 3.0 will implement the latest and greatest there is to offer including HMTL5, CSS3, RESTful APIs and more.