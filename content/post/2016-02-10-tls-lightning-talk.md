---
author: Greenjam94
categories:
- Uncategorized
date: "2016-02-10T16:26:19Z"
guid: https://blog.greenjam94.me/?p=269
id: 269
tags:
- blog.greenjam94.me import
title: TLS Lightning Talk
url: /tls-lightning-talk/
---

Hi everyone, last night I gave a lightning talk at Misec Jackson. It was a quick 15 minute summary of [my last blog post on TLS](https://blog.greenjam94.me/tls-what-is-it-and-why-it-matters/). I summed everything up into [12 slides](https://docs.google.com/presentation/d/1MAsplrzItVsfnRwnDFhSZl0p-wjl26y9A1IQd7rim2E/edit?usp=sharing) and threw in some last minute images to make it look better than just bullet points on bullet points.

## Other lightning talks from the night

I wasn’t the only talk that night, there was a talk on IPv6 that was pretty insightful. IPv6 is older then windows XP but it’s still not widely used. There’s been a couple hacks and misuse of it’s features already, but I’m sure there’s more to come. Another talk was on “[the evil bit](https://www.ietf.org/rfc/rfc3514.txt)“, network packets actually have a bit that can be set if the packet has malicious intent, and any security device should drop those packets immediately. Some website do this, others don’t. A fun idea would be hiding a service by ONLY accepting the inverse of that. The last talk of the night was about RFID tags and stealing card info, the speaker referenced a [talk](https://www.youtube.com/watch?v=ccAhVtvMki4) from DefCON 21 ([pdf of defcon slides](https://www.defcon.org/images/defcon-21/dc-21-presentations/Brown/DEFCON-21-Brown-RFID-Hacking-Updated.pdf)).

## My thoughts on my talk

<figure aria-describedby="caption-attachment-270" class="wp-caption alignright" id="attachment_270" style="width: 296px">![Look! I'm behind the podium!!](https://blog.greenjam94.me/wp-content/uploads/2016/02/Screen-Shot-2016-02-10-at-4.17.25-PM-296x300.png)<figcaption class="wp-caption-text" id="caption-attachment-270">Look! I’m finally behind the podium!!</figcaption></figure>

My talk went pretty well, I didn’t have any words on my slides. I had a lot of pictures that I used to replace my talking points and I wrote everything I wanted to talk about. This was my second time giving a presentation for hacking. My first attempt I was talking into a computer the entire night as I did a walk though. I feel like I did a lot better by not putting my words on the screen n reading off the slides, I was able to make more eye contact with the crowd and the words flowed more easily. If I was going to give this talk again, I’d do more research into the technical aspects of TLS MITM attacks, or how TLS is implemented. I had one question from the crowd about how someone would be able to decrypt a packet and I went into a MITM description… I feel like I might have misunderstood the question and I want to do more research with that before I present on the topic again. Let me know what you think about my slides, feel free to leave me a comment. The notes at the bottom of the google slides were my talking points, and most of those were summarizing my older blog post.

I’m glad to have a second set of slides under my belt. Now I have “Web hacking with the broken web app project” and “TLS: what is it and why it matters”. My next scheduled talk is on web app testing and will be at Misec Southfield in June. I look forward to giving back to the security community and I’m happy to be gain presentation skills as I learn more about information security.