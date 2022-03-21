---
author: Greenjam94
categories:
- Uncategorized
date: "2016-02-03T14:11:59Z"
guid: https://hackerunder.dev/?p=263
id: 263
tags:
- blog.greenjam94.me import
title: VTech Kid Connect Data Breach
url: /vtech-kid-connect/
---

On November 14, 2015, VTech discovered a hacker had broken into their databases, servers, and websites. The hacker used SQL injection to gain complete access to the databases that held all of the data used by the Kid Connect application that VTech uses.

A friend of mine wrote up an awesome case study about the breach and you can read it here [StephenManz\_KidConnectHack](https://hackerunder.dev/wp-content/uploads/2016/02/StephenManz_KidConnectHack.pdf).

## My two cents on the VTech breach

(Not a TL;DR of the case study, just what I took away)

While in the database, the hacker discovered that passwords were hashed using MD5. Which is pretty easy to crack now thanks to faster machines and better algorithms. MD5 hashes any input to 16 bytes of encryption, using brute force or [rainbow tables](https://en.wikipedia.org/wiki/Rainbow_table), even guessing common passwords and comparing them to the hash, are great ways to break the hash.

This means the hacker gained access to customer data, related information like who ordered the toys, and information related to that. For example credit card info, home addresses, and more.

This is another example why it’s really important to consider abuse cases when testing you applications. Even if your databases is “just for a kids app” it can still be the end of the world for your clients. **Proper input validation and query construction is very important for any application**. Automated test cases should try things other than what is expected, like SQL injection or XSS. For example, a child’s name shouldn’t contain special characters.

<figure aria-describedby="caption-attachment-264" class="wp-caption aligncenter" id="attachment_264" style="width: 666px">![Exploits of a mom, xkcd #327](https://hackerunder.dev/wp-content/uploads/2016/02/exploits_of_a_mom.png)<figcaption class="wp-caption-text" id="caption-attachment-264">Exploits of a mom, xkcd #327</figcaption></figure>

Also don’t use algorithms that are broken, at the very least google the algorithms you plan on using and see what the infosec community thinks of them. MD5 hashes and SHA-1 encryption are examples of algorithms that used to be good, but have since been become crackable. **Do your research!**

Again, please check out Stephen’s case study in the link above, he did a great job summarizing the VTech breach. VTech also has a [formal press release](https://www.vtech.com/en/press_release/2016/faq-about-cyber-attack-on-vtech-learning-lodge/) about this breach.