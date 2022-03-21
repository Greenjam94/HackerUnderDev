---
author: Greenjam94
categories:
- Uncategorized
date: "2015-10-24T21:17:59Z"
guid: https://hackerunder.dev/?p=139
id: 139
tags:
- blog.greenjam94.me import
title: Assembling a Delta 3D Printer
url: /assembling-a-delta-3d-printer/
xyz_smap:
- "1"
---

Hey everyone, it’s been a while since I’ve written something about hardware. I’ll share a current project of mine that has taken some interesting turns. Something that is all the age right now is 3D printing. Most assembled printers range from $400 to $800 and that can range on a lot of things from filament type to the hardware in use. Kits generally run cheaper, because you have to assemble them yourself. A coworker found [a really nice kit at a cheap price](http://folgertech.com/collections/3d-printer-full-kits/products/folger-tech-kossel-2020-full-3d-printer-kit), so my dad and I jumped on the offer. There’s two kinds of 3D printers that use only 3 axises, one that moves how you’d expect in a square grid fashion with (x, y, z) coordinates. The second kind is called a delta 3d printer and uses three arms to print an object within a cylinder shaped area.

## First Attempt

When I first got everything shipped to my house, I had already filled through the directions online. Everything seemed simple enough, it was just some nuts and bolts, plugging in electronics, and a touch of soldering. However when I went to put it together after pulling everything out of the boxes. I found that I didn’t even have the right tools! so I repacked everything and had to wait for the weekend so I could take it home to work on it with my dad.

## Second Attempt

My second attempt went a lot smoother, mainly because my dad was there and has every tool I needed. He was also able to help me decipher the instructions because as easy as I thought it was. The whole build took two days of straight work to get it fully assembled. The printer looks great fully assembled.

The first thing the instructions say to do after assembly is to calibrate the motor controls using an acrylic wrench. On the third motor controller, While we were adjusting it, we saw smoke coming out of the back. My dad and I quickly turned it off and didn’t touch it after that. I tried to connect my computer to the Arduino that runs the controllers and I wasn’t able to get it to register on my Mac.

I made a [reddit post](https://www.reddit.com/r/3Dprinting/comments/3mttue/need_help_with_folger_kossel_2020_build/) to r/3Dprinting and was told I had a common case of magic smoke. Basically I fried the electronics and I can’t fix it so I need to go buy a new set of boards and try again, hopefully not frying everything this time. However, I can’t find any damage on the boards from the smoke.. I’m not even sure what started to smoke!

## What’s Next for the Delta 3D Printer?

You can see photos of my current progress [here](https://drive.google.com/open?id=0B-8f8pBGcJiHM3p4Z1A0X0VFejA).

Right now, I want to avoid buying a set of [replacement electronics](http://www.aliexpress.com/item/Free-shipping-Mega-2560-R3-1pcs-RAMPS-1-4-Controller-5pcs-A4988-Stepper-Driver-Module-USB/32232368294.html?spm=2114.01020208.3.13.2wUpaH&ws_ab_test=201556_6,201527_3_71_72_73_74_75,201409_5) if possible. As soon as I can, I want to remove the shield and motor controls so I can test the Arduino by itself. If that’s still unresponsive, then i guess it’s safe to assume that’s fried and I’ll need to buy a new board. However I’m not sure how to test the shield or controllers by themselves. I might just have to buy a whole new set of electronics and get them working soon. I really want to see how this printer works.

The first thing I want to build is replacement parts for my printer. About a third of the delta 3D printer is made out of printed parts. It is smart to have spares on hand in case something wears out or breaks. I’d also like to build extensions like a mount for a larger fan, or possibly wire clips that fit into the 2020 frames. One day I want to use this printer to build a lot of cool knick-nacks for around the house and for everyday use, like a money clip or a headphones holder so they don’t get tangled. My coworker uses his to build [prosthetic hands](http://enablingthefuture.org/upper-limb-prosthetics/the-raptor-hand/) for a non-profit group, that would definitely be something I’d be interested in building at some point.