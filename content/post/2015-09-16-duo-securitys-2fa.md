---
author: Greenjam94
categories:
- Uncategorized
date: "2015-09-16T16:57:12Z"
guid: http://blog.greenjam94.me/?p=127
id: 127
tags:
- blog.greenjam94.me import
title: Duo Security&#8217;s 2FA
url: /duo-securitys-2fa/
xyz_smap:
- "1"
---

I hope you’ve been enjoying my posts. I know that writing these posts have been a good outlet for all I have learned over the last few years. This website is hosted on a VM, but it’s still a server that’s vulnerable to your every day hacks. For instance, every day someone pings my server, finds the SSH port and attempts to brute force into it. Now while there’s nothing here for them to steal, there’s still 20GB of free internet storage for whatever they want and the only thing stopping that brute force attack is that they can’t guess my password.

This is where Two Factor Authentication (2FA) comes into play. 2FA is where a user presents a password (first factor) and a key (second factor) for authentication. That key is usually a randomized code that is generated from secure app or hardware device like a USB drive. An attacker can guess that your password is scriptKitty but they can’t guess a randomly generated key. So to prevent someone from gaining access to my VM I’m setting up 2FA using Duo.

Why Duo? Well honestly they were recommended by my mentor and they also sponsored a2y.asm. So that’s two brownie points for them. But the real reason I went with them is because 2FA is their bread and butter. Duo already has mobile apps, APIs, and tons of documentation that allow anyone (even a college student) to set up high quality 2FA in under an hour. Not to mention that Duo’s mobile app helps prevent social engineering attempts or other 2FA hacks (Yes, people still hack 2FA even though I said you can’t guess the key).

I sleep more soundly at night knowing that no one will be able to brute force my server thanks to Duo. However that’s not the only thing I want to do to my server. I want to run through the other services being used on my server and make sure everything is locked down tight. I also want to customize Duo’s login process. For one thing, when the root account tries to log in there is a message that comes back saying something like “Register here www.api.duo.com/###…” and I don’t like that because I know people are using the root account for brute force. I’d prefer to remove the url from the message entirely. So that only I have access to the api and no one is trying to break the 2FA.