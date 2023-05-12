---
author: Greenjam94
categories:
- Tools
date: "2015-02-13T13:57:50Z"
guid: https://hackerunder.dev/?p=50
id: 50
tags:
- virtualbox
- owasp
- kali
title: 'Step 3: Set Up Your Hacking Environment'
url: /your-hacking-environment/
---

Everyone wants to break into their neighbors wifi or steal someones password at Starbucks, but depending on National, State, and local law, even packet sniffing could be illegal. So how do we safely practice how to hack before we are ready to find Sony’s back door? We set up a environment for virtual machines on our local computer or server!

For those of you who don’t know what a Virtual Machine is, it’s a “computer” inside your computer. Using programs like VMware or Oracle’s VM VirtualBox (which is free) you can have multiple systems running on your computer depending on your computer’s RAM and processing power. I suggest you download VirtualBox to get started: <https://www.virtualbox.org/wiki/Downloads>

[![VirtualBox view of environments](https://hackerunder.dev/wp-content/uploads/2015/03/ss1-300x224.jpg)](https://hackerunder.dev/wp-content/uploads/2015/03/ss1.jpg)

After you have that installed, you need to get the operating systems that’ll make up your VMs. I suggest using Kali for your “hacker” machine and OWASP’s BWA for your “victim” machine. OWASP is a open source community for watching web application security. You should check out[http://www.owasp.org](http://www.owasp.org/) to learn more about them. Be sure to check out their top 10 vulnerabilities for websites. You can download the files for both VMs at the links below. Special note about the BWA VM: It’s made of VMware files, there’s no installing like you would with Kali. Be sure to use an existing harddrive and select one of the files from the .zip folder you downloaded.  
<https://www.kali.org/downloads/>  
[http://www.slideshare.net/michael\_coates/lab-setup-28126110](http://www.slideshare.net/michael_coates/lab-setup-28126110)

Follow these tutorials to get your systems online![  
Installing Kali as a Virtual Macine](https://forums.kali.org/showthread.php?74-Installing-Kali-Linux-as-a-Virtual-Machine-in-Virtual-Box)[  
Installing OWASPs BWA as a Virtual Machine](http://oscarhjelm.com/blag/?p=1295)

If you’ve never used a linux operating system before, I suggest you learn fast! Check out how to use the bash commands (terminal) and learn some of the tools that Kali has to offer.

[![Kali environment](https://hackerunder.dev/wp-content/uploads/2015/03/ss2-300x244.jpg)](https://hackerunder.dev/wp-content/uploads/2015/03/ss2.jpg)

Now, your “victim” is specifically made to have vulnerabilities! Its up to you to find them, or if you want more of a step by step then I suggest you google how to get in or check out the [BWA project files](https://www.owasp.org/index.php/OWASP_Broken_Web_Applications_Project)

[![OWASP BWA environment](https://hackerunder.dev/wp-content/uploads/2015/03/ss3-300x91.jpg)](https://hackerunder.dev/wp-content/uploads/2015/03/ss3.jpg)

Confession: While writing this blog I got OWASP BWA working on my Windows machine for the first time. I’m very excited to try it out!