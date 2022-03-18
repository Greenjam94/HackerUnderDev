---
author: Greenjam94
categories:
- Uncategorized
date: "2018-12-16T04:08:56Z"
guid: https://www.hackerunder.dev/?p=666
id: 666
tags:
- blog.greenjam94.me import
title: My experience with RuCTFe 2018
url: /my-experience-with-ructfe-2018/
---

This year was my fourth attempt at the RuCTFe competition. I was leading the #misec team this year along with some smart and talented people.

For those who are unaware. The RuCTFe is a Russian capture the flag event, held online and open to everyone around the world. This year’s event was in November and it was my first time leading the team.

The CTF is an active “red vs blue” game, where each team is given a server of vulnerable applications. It’s a team’s duty to defend and attack. Defending as in patching their own copy of the applications while allowing Russia to provide them with flags. Attacking by exploiting the vulnerabilities found on the applications to exfiltrate flags and submit them for points. This event is great for web application security practices such as penetration testing, reverse engineering, software engineering, and other related skills.

# How I wound up leading #misec’s team

I started competing with misec three years ago in 2015. I was new to CTFs and needed a lot of help to understand anything and everything about what was going on. Through the event I made a lot of great connections with people in the community who would end up teaching me so much over the next few years.

I always made it a point to volunteer and help however I could to make the event happen smoothly and successfully. In 2017, I provided some training material and tried to walk some college students through some of the steps I’ve done in the past.

This year, I was offered to take more of a leadership role. The team this year was very close and it made organization easy. We had a smaller group, so breaking into teams and defining roles wasn’t a major concern for me. My main goals were to have a location, clearly defined goals for the event (what people could be doing), and making sure we had access to all necessary hardware (routing, switches, and a server with virtualbox).

# How the competition day went

The night before the event started, I met the other leadership where we were setting up the equipment. We got the machines online and tested, even if it took 5 hours, before the event started. This is huge considering the previous years I’ve attended there’s been networking issues that always resulted in us starting late.

The event kicked off at 5am EST, we got the game VM image and unlocked it. First thing we had to do was boot into single user mode, reset the root password and get the services online. This took a few tries but we were able to do it before the “attack phase” started at 6am.

After that I attempted to have a quick meeting with all the attendees, I went over the single user mode process so we could have people looking at the applications offline. Everyone seemed to have an idea of something to attempt and got to work quickly.

I was the first one on the team to find flags on another team. One of the applications was based on IRC and I was able to use hexchat on my Kali machine to view the chats. Each team had a IRC server and on occasion, there would be a user who would send a message containing a flag. I quickly started to manually copy and paste these flags into a rough script that would submit the flags for points.

I spent the rest of the day trying to follow up with others and help where I could, but most of my attention was on submitting the flags. Unfortunately we were unable to write a script that could automate flag retrieval and submission.

Near the end of the day, another team member found out how to steal flags from another application and was able to turn those in for points.

All in all, #misec finished in the top third of all registered teams. I’m happy with those results even if it wasn’t as high a ranking as previous years. It’s a personal achievement to find a method of stealing other users flags and being able to lead such an awesome group of hackers in a fun event like the RuCTFe.

# What I’d like to see for next year

This was my first time leading the team, and it was pretty rocky. We had a single preparation meeting to discuss getting hardware and what the event was. In the future, it’d be good to review previous years’ challenges as well as discuss what kind of activities would be involved.

As a leader, I think it was a personal failure to focus as long as I did on “playing”. While it was cool to get a flag, it would’ve been better to focus on the group and make sure everyone had a task, was communicating, and that they felt like they were contributing or at least was working with someone that allowed them to learn.

We had a lot of interested members this year, but not as experienced as previous years. I think this was another factor that could’ve been helped with more training and preparation.

Communication is key. It is all to easy to be sucked into a rabbit hole or get too nervous to ask out for help. It’s important to share thought processes, ask questions, and work with the team. This is something leadership should ensure is happening as we all know it’s too easy to forget.

I’d like to see groups in the team based on the applications. Instead of “ok everyone, pick something you like and swing at it”, gather people who are working on the same thing and have them in the same area. If they get stuck, need help, or want to share a break through, it’s much easier when you know who to talk to.

Better statistics on game network traffic would be amazing. If we could see the contents of each packet going to and from our server, it could be relatively easy to find flags and reverse engineer how to replicate the exploits. This may be hard to implement since it’s involves changing how the team connects to the game network, and that’s been an issue in the past for our team.