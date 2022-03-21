---
author: Greenjam94
categories:
- Uncategorized
date: "2015-12-11T14:40:05Z"
guid: https://hackerunder.dev/?p=219
id: 219
tags:
- blog.greenjam94.me import
title: My First Presentation &#8211; Web Hacking
url: /web-app-hacking/
xyz_smap:
- "1"
---

Tuesday December 8th was the last meeting of the semester for [Spartan Hackers](http://spartanhackers.com) and I gave the presentation. A group of students at Michigan State University who go to hackathons and want to learn more about computer science. Each week we have workshops to introduce new things to our members, topics vary from “Intro to HTML” to “Web Scraping”.

The original idea was to have a security company come in and talk with us, but that fell through at the last minute. The next idea was for me to beg one of the Misec members to give one of their talks but I wasn’t able to get to them soon enough (I only gave them around a weeks notice to prepare something). So the backup backup plan? I’ll present a security topic! After all I am a Misec member and I know what I’m doing, right?

So I grabbed my computer, downloaded OWASP’s BWA project and starting searching for vulnerabilities that I could show people. I did my best to stick to the OWASP’s top 10. I found examples of XSS, SQL injection, Directory Browsing, Broken Authentication, and Direct Object Reference. A lot of the heavy hitters for web app hacking. So with my examples in hand, I set out to make a presentation. It was planned to be short, I didn’t want to talk the entire time. I wanted it to be interactive. So when we sent out the invite we told people to download OWASP BWA and virtual box, I even had step by step guides on a [previous blog post](https://hackerunder.dev/exploiting-owasp-bwa/). The idea was to discuss a topic, show people, let them play, then repeat the process with the next hack. Sounds awesome right?

<figure aria-describedby="caption-attachment-220" class="wp-caption aligncenter" id="attachment_220" style="width: 300px">![Peruggia Hedgehog from presentation](https://hackerunder.dev/wp-content/uploads/2015/12/lolhax-300x225.jpg)<figcaption class="wp-caption-text" id="caption-attachment-220">Yeah! If people downloaded BWA before coming!!</figcaption></figure>

I got to the meeting an hour before it started, I opened all the files I needed. I had my cheatsheet of hacking demos, the blog posts, my slides, and the VM all ready to go. I ran through each hack to make sure they worked and was already getting nervous before anyone even showed up. Pizza came, the eboard showed up, and a few members strolled in. We all talked for a bit, I posted on twitter about the presentation. As the night was about to start, the club president asked how I was doing. I said it’d go great if everyone got things downloaded in time and the reaction I got back didn’t really help my nerves. I should have guessed, but only a few people had the vm installed. My plan of show and tell, then let people find their own examples came crashing down. I had about 30 mins of slides and the meeting was supposed to be for an hour. Should be easy, just double the length of each slide, talk slowly… yeah.. ok.

I think the actual presentation went well, besides not being as interactive as I wanted I was able to show off all the hacks I wanted. I wasn’t as engaging as I probably could have been, I was always behind the podium writing the next hack. When I asked for questions, most of the time it was quiet. I managed to go for 40 minutes before the slides ran out, I even gave a very rough “demo” of using [armitage ](http://www.fastandeasyhacking.com/)to find simple bugs.. but I stumbled through that.

Feedback that I got afterwards was that there was a lot of information. All if it was cool, seemed surprisingly simple, but there were a lot of topics that I just scratched the surface of. People seemed interested in a more in-depth workshop. Maybe [Spartan Hackers](https://twitter.com/SpartanHackers) will get a group together to run through the BWA some time. The other idea I had for a future event was a Q&amp;A panel with local hackers, maybe instead of a lecture style presentation, people walk around to hackers to learn about things in a more open setting.