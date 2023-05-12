---
author: Greenjam94
categories:
- Community
date: "2017-01-22T19:42:01Z"
guid: https://hackerunder.dev/?p=530
id: 530
tags:
- conference
title: How I got to Shmoocon2017
url: /getting-to-shmoocon2017/
---

[Shmoocon](http://shmoocon.com/) is a hacker conference in Washington DC. I’ve been interested in going since 2015 but this is the first year I’ve been able to make it out. The conference was really hard to get into. Not because it’s expensive or that it’s hard to get to DC, but because the process to get my ticket was a unique challenge in itself. It required me to rely on good friends, new skills, and a whole lot of luck.

## Trying for a badge

I roomed with [@infosystir](https://twitter.com/infosystir) for the weekend, we saw an awesome deal on flights and rushed to get tickets and the hotel settled away. That was the easy part. Getting Shmoocon tickets was the worst experience I’ve dealt with compared to other conferences. There were three “rounds” of people rapidly refreshing the tickets webpage. Each time, I failed to get one. While @infosystir had the connections to score a media badge, I was bound to attend lobby con.

For those who don’t know, lobby con is where non-badge attendees settle in at the hotel bar and network with others who were able to attend. Badges usually float around from person to person. More than a few last minute cancellations are made each year, so people have extras as well. It is better to attempt to social engineer a ticket then to cancel a flight and lose any deposits. Either way I wasn’t going to bail on a conference.

## Starting out right

Thursday night before the conference started, @infosystir and I set out for the bars. Before long, we met up with [@lintile](https://twitter.com/lintile) and he told me about an extra ticket. There was just one problem, it was a prize to a small cryptochallenge he made. On twitter, there was a post with random characters and a #shmoocon tag. Someone had responded that they ended up with gibberish after a failed attempt. At first I was worried that I could not beat the challenge before Shmoocon started. Even if the person on Twitter was joking, I’ve never tried a cryptography challenge before.

## Step 1 – Decoding

As we sat at the bar, I asked @lintile where to start. He asked [@TheSweetKat](https://twitter.com/theSweetKat) what it meant to have a message that ended with “==” and her immediate response was “it’s base64 encoded”. I quickly pulled out my phone and decoded the string, the answer I got was “&lt;to be added here&gt;”. Great another task, of course it would not be that easy.

I overheard @lintle mention md5 hashes so I looked that up next. It’s safe to assume that if the hash is 32 characters long, that it is MD5 or something similar to MD5. Thirty-two characters at least narrows it down to a handful of options, rather than a ton of options. So that’s what I started on next. My phone wasn’t powerful enough to brute force a hash, it was a Samsung S4 with a dying battery. However, after the conference I found there is an android app called [Hash Suite](http://hashsuite.openwall.net/android) so it is possible for phones to crack some md5 hashes.

## Step 2 – The hash

While I was desperately googling for online hash cracking websites, I reached out to a experienced friend who would know where to start. My googling skills failed me, but [@ashioni](https://twitter.com/ashioni) did not. He was able to get on his laptop and start up hashcat to start guessing strings that would result in a matching hash.

<figure aria-describedby="caption-attachment-532" class="wp-caption alignright" id="attachment_532" style="width: 300px">[![Lintile's tweet with the encoded hash](https://hackerunder.dev/wp-content/uploads/2017/01/Screen-Shot-2017-01-22-at-5.35.15-PM-300x112.png)](https://hackerunder.dev/wp-content/uploads/2017/01/Screen-Shot-2017-01-22-at-5.35.15-PM.png)<figcaption class="wp-caption-text" id="caption-attachment-532">The hash that started it all and the first hint.</figcaption></figure>

We came to the correct answer by using OSINT research.

OpenSource Intelligence leverages publicly available information, in this case @lintile’s Twitter page, to gather information and generate a profile of a target. Target profiles can then be leveraged in many ways. Providing better word lists or giving hints to crack a code are a few examples. In this case the target profile was used to come up with possible passwords the target may be using. We were able to narrow the string down to be something with only 10 lowercase letters and contained “@shmoo”. “?l” is a [hashcat](https://hashcat.net/wiki/doku.php?id=hashcat) variable for lowercase letters. In order to guess the string that made the hash we were trying”?l?l?l?l@shmoo”. @Ashioni’s laptop should have been able to crack this within an hour but for some reason, there were no matches by the time my phone died later that night.

## Cracking the code

I woke up the next morning and struggled to think what else I could do. @Ashioni had started up his password cracking rig that can do roughly 10 billion MD5 bruteforce attempts per second. Yet still no luck. I wanted to help, but I didn’t have hashcat on my mac or a connection to download the tool. While trying to think what else was possible; I was lucky to find out that it’s possible to hash strings using terminal on mac.

<figure aria-describedby="caption-attachment-531" class="wp-caption alignleft" id="attachment_531" style="width: 300px">[![terminal output from hashing strings](https://hackerunder.dev/wp-content/uploads/2017/01/Screen-Shot-2017-01-13-at-1.43.30-PM-300x123.png)](https://hackerunder.dev/wp-content/uploads/2017/01/Screen-Shot-2017-01-13-at-1.43.30-PM.png)<figcaption class="wp-caption-text" id="caption-attachment-531">These are some of my guesses.. The last hash on the bottom is the hash from the challenge.</figcaption></figure>

I started guessing random 4 letter works that @lintile might have used. Failure after failure, the hashes I made didn’t match. Free, move, goto, tick, cryp… none of them were working. It wasn’t until I checked @lintile’s Twitter again that I thought to use his handle truncated to 4 letters. the hash of “lint@shmoo” was as close as I got to matching the hash, but I had a “off by one” error. The last character of the hashes didn’t match. I tried capitalizing the L, I tried “tile” and other combinations of @lintile. Each of those created hashes with entirely different hashes. Nothing was as close of a match as “lint@shmoo”. When talking to @ashioni about the cracking rig not being able to find a match and my guess being so close. We though that using CTRL-C to copy may have been the culprit for the spelling error.

At the same time I figured this out, @lintile reached out to me and said I could have his second badge, the conference was about to start and I was the closest to cracking the hash. When I met up with him, I asked if “lint@shmoo” was correct and he said yes. I was ecstatic! Cracking the code and getting it right felt great. Wait… what about the last character of the hash? As it turns out, it was just a typo when copying the hash into the base64 encoder. That’s why @ashioni’s hashcat brute force attempts never matched.

## Success!

It was really cool to get a Shmoocon ticket by completing a crypto challenge. Attending shmoocon wouldn’t have been possible without @infosystir, @lintile, and @ashioni. I really enjoyed completing my first crypto challenge as well. I talked to @lintile throughout shmoocon and am looking into more common ciphers and ways to practice for challenges in the future. He creates challenges for fun and also runs the Circle city con CTF and I’m looking forward to that. [rumkin.com](http://rumkin.com/) is a website he shared with me to learn about some other common ciphers… I think that in order to practice them, I’m going to try and create a little webpage with a simple crypto challenge.