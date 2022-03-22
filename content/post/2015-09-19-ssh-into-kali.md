---
author: Greenjam94
categories:
- Uncategorized
date: "2015-09-19T17:30:18Z"
guid: https://hackerunder.dev/?p=129
id: 129
tags:

title: SSH into Kali
url: /ssh-into-kali/
xyz_smap:
- "1"
---

I’ve had a couple posts about [Kali](https://www.kali.org/) on here already. But I still haven’t had a chance to fully get in to it myself. I know, it’s tragic right? Well for those who know less than I do about it; Kali is a linux distro from Offensive Security that comes packed with tools and programs that make hacking easy. However carrying around a computer for work, one for class, one with Windows, and a tablet or two isn’t really an option, unless your bag is designed for 80lbs. A quick fix for that is to allow remote connections between other computers. My infosec mentor suggested this idea so I could have easy access to the [tools](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwjPsauWiKXJAhUXm4gKHaIGDWkQFggeMAA&url=http%3A%2F%2Ftools.kali.org%2F&usg=AFQjCNELK44Nma0gAjAwY1_Yv-fuJtYs1A&sig2=t7xbdD6Zu3JpuUhIFXIwvw) available on Kali.

## Setting up SSH

I want to set up an SSH port on my kali box so I can access the tools from anywhere I have an internet connection. Currently I have Kali on an old windows computer that used to run vista. I really don’t want to carry that around. However I always have my macbook air for classes so I can use that and then connect to my other, heavier, computer. Setting up SSH on a kali box is really easy, I followed [one of many tutorials](http://www.blackmoreops.com/2014/06/19/kali-linux-remote-ssh/) online and it all seems to work when i run ssh user@localhost. I even set up some cool ascii art to personalize the process of logging in. I had fun with that part.

## Continuing Issues

The complicated part is my router from Comcast. For some reason I cant set up correct port forwarding so outside SSH traffic can get to my laptop. If anyone can help me figure that part out, I’d owe you one! At first I thought it was because I have a router behind a router on my network. but even after fixing that and moving to the outter network. I still can’t get port forwarding to work and it seems that it’s because Comcast doesn’t like having their rental equipment used as a network bridge. I’m not entirely sure if those reports are true; but after struggling for so long with this and not being able to get it working I’m starting to assume it’s true.