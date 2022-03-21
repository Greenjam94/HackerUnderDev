---
author: Greenjam94
categories:
- Walkthroughs
ct_period_last_updated:
- default
date: "2016-05-15T11:30:55Z"
guid: https://hackerunder.dev/?p=333
id: 333
tags:
- blog.greenjam94.me import
title: 'OverTheWire: Bandit'
url: /overthewire-bandit/
---

Hey everyone, this post about Bandit is NOT a walkthrough of the greatest (only) “learn bash hacking” programs I’ve completed. This is NOT going to give you an advantage if you’re looking for cheat codes. This post will hopefully make you click on [OverTheWire](http://overthewire.org/wargames/) and want to try it out for yourself.

## Why you should try Bandit

Do you work with Linux, bash shells, scripts, or ever have to deal with the command line? If you are a developer, network admin, forensic analyst, incident responser, pentester… or any other IT job, the answer is most likely yes (unless you have some serious automation or “a guy” for that). Whether you’re entering into a new field or you need a refresher course, Bandit is the first of many war games offered by the good looking hackers of OverTheWire. Start at [Lesson 0](http://overthewire.org/wargames/bandit/) and work your way through them all.

Last night, I met up with a group of fellow hackers from [\#misec](https://misec.us) and we tackled it. We went from 4pm to 12am, only stopping for a taco/wings run. We had a wide range of skill levels from 15 years of experience to a recent college grad, but we were able to go through the tasks at a pretty even pace. Doing this training in a open group where everyone discusses their tactics was really cool because **there are multiple ways to do the same lesson**, there’s never one right answer. I highly suggest you do the same. Get a group of 4-10 people, grab a six-pack and hunker down somewhere.

## Helpful Hints

By the end of the night, I had expanded on the bash commands I already knew like ls, cat, chmod, mkdir, touch, openssl, and vi/nano/vim. I looked at the man page (help documentation) for the first time for other commands I heard of but didn’t use: grep, file, diff, gzip, tar, and so much more. Seriously guys and gals, you will not complete this course unless you type **&lt;cmd&gt; –help** or **man &lt;cmd&gt;**.

There was only really tricky lesson in Bandit for those unfamiliar with development or python. So to assist but not give the answer away, I’d like to point a few things out about python. Please note this is one specific way to beat this level, [@jadedtreebeard](https://twitter.com/jadedtreebeard) found a faster way to beat this level without even touching python.

- Run python scripts by writing: **python filename.py**
- Variables have type, so numbers (30002) are integers and words are strings (“words”) 
    - Change integers to strings: **str(myVariable)**
    - Change strings to integers: int(myString)
- Importing packages are the first thing to do in a .py file 
    - I suggest you look at [socket](https://docs.python.org/2/library/socket.html) \*COUGH COUGH\*
- **range(x, y)** will give you a *list* starting at x going to y
- For loops will loop through every object in a list 
    - Syntax: **for something in** *list***:**
    - Indent under that line and it’ll be included in the list
- If statements are powerful 
    - What would happen if you only did something when a variable contained a certain substring 
        - **if** only **“Correct”** was **in someString:** then I could **print someString** only when it has the right values instead of every incorrect one as well… **😉**

There are 27 lessons in Bandit, it took our group 8 hours to casually and thoroughly go through every lesson. A few are very tricky. I suggest you a) read cmd manuals b) read the associated links from OverTheWire for each lesson c) brainstorm and bounce ideas around your group. The only thing you should not do is google the answer, this is a public activity and other people have already done this. I suggest you stay away from googling “how to complete Bandit”…. It’s not cool, you can learn so much more by following a-c.

Lastly, I want to give a shout out to [@Ashioni](https://twitter.com/Ashioni), [@JadedTreeBeard](https://twitter.com/jadedtreebeard), [@bigryanb](https://twitter.com/bigryanb), [@EquinchOcha](https://twitter.com/EquincuOcha) and the other hackers in my group who’s twitter handles I do not know… It’s because of them I had such a fun time instead of pulling my hair out when I got stuck on lesson 28. If you are in the Michigan area, you seriously need to look up [\#misec](https://misec.us), it’s a great group of people. Reach out to [@Ashioni](https://twitter.com/Ashioni), he frequently puts on events like this to help others learn hacking skills.

After you’ve conquered Bandit, move on to the next level: [Leviathan](https://hackerunder.dev/overthewire-leviathan/). I suggest trying Bandit in a group with other people, but Leviathan should be pretty tame and is a good way to test your individual skills.