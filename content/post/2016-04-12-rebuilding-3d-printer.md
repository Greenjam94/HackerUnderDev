---
author: Greenjam94
categories:
- Uncategorized
date: "2016-04-12T12:56:46Z"
guid: https://blog.greenjam94.me/?p=316
id: 316
tags:
- blog.greenjam94.me import
title: Rebuilding the 3D printer
url: /rebuilding-3d-printer/
---

A long long time ago, I wrote a blog post about trying to assemble a new [Folger Tech](http://folgertech.com/collections/3d-printer-full-kits/products/folger-tech-kossel-2020-full-3d-printer-kit) 3D printer. Long story short, I was given a bad Arduino board that started to smoke as soon as it was plugged. I spent weeks trying to get help from Folger to check my wiring, because to the best of my knowledge I had followed their instructions and I didn’t know what was wrong. Well after getting a response from them saying “Looks fine to me” and Reddit users not being able to help I looked for a cheap replacement for the board and found the electronics from an online store that shipped from china. I got a new board, shield, and 5 motor controls for under $20.

## Reassembling the 3D printer

After a couple months of procrastination, I finally decided to rip apart the printer and install the new electronics. This wasn’t the first time I had to disassemble part of the printer, tear apart the electronics and reassemble them. The only good thing from that is practice makes perfect. This is the fourth time having to go through the process and that makes it easier. It’s also nice to have a second set of hands, I had a helper who was able to catch falling screws, hold wires, and check the instructions when my hands were full.

Before I started doing anything to the printer, I re-watched a video on Youtube of a guy that assembled the same printer and got it working. Even when I was following along with the video, I didn’t do anything different, so I’m pretty sure the only issue was the bad board.

<iframe allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/eRbcLqQbssQ?feature=oembed" width="500"></iframe>

Once I got everything reinstalled, plugging it in was the most nerve racking part of the entire process. I didn’t want to have it start smoking again because I really did wire in something wrong. Luckily I had nothing to fear, plugging it in didn’t cause anything to smoke and that was a huge relief to me. I was able to calibrate the motor controls and nothing started to burn even as it was on for around 15 minutes.

## Firmware/Computer Software

The next stage was getting the firmware installed on the Arduino. Another rookie mistake I learned. Install the firmware before you connect everything together. The firmware (and configuration) was provided by Folger Tech and for some reason I couldn’t install it from my mac once everything on the 3D printer was connected to the board. The computer knew there was a serial connection, but couldn’t do anything with it. After reinstalling drivers and googling fixes, I finally gave up and decided to try another computer. I installed the [Arduino IDE](https://www.arduino.cc/en/Main/Software) and [Repitier Host](http://www.repetier.com/) onto my new windows 10 “gaming” laptop that I just got for Christmas and hoped that would work. Luckily it did, I was able to upload the firmware without having to disassemble the printer for the fifth time.

I was able to connect to the printer using Repititer. I set up the software’s configuration and tested the end stops on the printer. It seemed like the printer and computer was working well, so I tried to send the “go home” command to the printer, so the nozzle would move to the default position… Well, guess what happened.

<https://www.facebook.com/Greenjam94/videos/10156860784315201/>

## What’s next?

This isn’t the end, I’m currently messing with more configurations for the 3D printer and the Repitier software, I’m not sure what I need to do next but I’ll figure it out eventually. Hopefully sometime soon you’ll see a post with my first actual print.