---
author: Greenjam94
categories:
- Tools
date: "2015-07-29T23:15:37Z"
guid: https://hackerunder.dev/?p=106
id: 106
tags:
- IRC
title: Found a group, sticking with it.
url: /setting-up-irc/
xyz_smap:
- "1"
---

[GrrCon 2015](http://grrcon.com/) is in October, it’ll be a great conference with a lot of talks. It’s the first con I’ll be able to attend. The tickets are a little expensive and I was unsure about going since this would be my first conference… Not to mention I’m still trying to get through college and I’m tight on money. So of course, I’m volunteering! I’ll be working my butt off to get you the best Con possible while making all the connections I can. I shared this on Twitter and my post was noticed by a [\#MiSec](http://michsec.org/) member and I was invited to a social night. MiSec is the first real hacking group that I’ve been able to attend in person. At the social, I met new people and they showed me a lot of things. I picked a few locks, heard stories of previous cons and some awesome presenters. I was inspired to give this group my all, while learning all I can. This is a valuable stepping stone on my way to becoming a hacker.

If you read my sappy intro, then you’ll be happy to getting to this part. #MiSec has a IRC channel on irc.freenode.net and there’s many ways to get on that channel. There’s a lot of people there who can teach you a lot but beware it can get a little spicy. A quick and simple way to log on is on [irccloud.com](http://www.irccloud.com). I was on in about an hour and meeting more people.

One of the members helped me set up irssi on my Virtual Machine with screens. This website and [greenjam94.me](http://greenjam94.me) is hosted on a little box that wasn’t getting much use besides web hosting. Now, it’s continuously connected to the IRC channel and logging every single post. Which I find awesome, I’ll never miss a single tip or trick! (or so I hope).

Setting it up was was really easy, I followed along with this [beginner guide](http://www.irssi.org/beginner) which was useful. It also helps to be in touch with someone who’s done it before. After that, it’s all set up, I can SSH into my server from anywhere. type “screen -rd” to reconnect to the permanent IRC connection and start talking to all my new buds.

Some commands I want to remember for this:  
**Screen**  
$ screen -rd – reconnect to last screen  
ctrl-a # – go to screen #  
ctrl-a c – create screen  
ctrl-a k – kill screen  
more found here at: [rackaid](http://www.rackaid.com/blog/linux-screen-tutorial-and-how-to/)

**irssi**  
/win # – Go to win #  
ctrl+n – next window  
ctrl+p – previous window  
/LAYOUT SAVE – save windows  
/SAVE – save default theme  
more at: [irssi docs](http://www.irssi.org/documentation/manual)