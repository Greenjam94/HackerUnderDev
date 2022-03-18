---
author: Greenjam94
categories:
- Uncategorized
date: "2017-09-13T17:06:04Z"
guid: https://blog.greenjam94.me/?p=625
id: 625
tags:
- blog.greenjam94.me import
title: Installing Kali and Metasploitable on VirtualBox
url: /installing-kali-metasploitable-virtualbox/
---

Have you ever wanted to be a 1337 hacker like you see in the movies? Metasploit automates some of the harder tasks related to penetration testing. This blog post is quick setup to install two virtual machines that will allow you to explore how to use Metasploit.

## Step 1: Get files needed to create the VMs

- Download [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- Download [Kali](https://images.offensive-security.com/virtual-images/Kali-Linux-2017.1-vbox-amd64.ova) for VirtualBox
- Clone [Metasploitable2](https://sourceforge.net/projects/metasploitable/files/latest/download)

## Step 2: Setup Kali

Open VirtualBox, click File &gt; Import Appliance. Choose the kali.ova file that you downloaded from the link above. Click continue to review the VM settings. Hit import, none of the settings should require changing. The import will take a few minutes to complete depending on your machine.[![](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-3.01.46-PM.png)](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-3.01.46-PM.png)

If the VM fails to start after import, read the details of the failure. If it’s related to USB emulation then change the settings. Open the VM settings by right clicking the VM. Click settings. Find the ports tab and click USB. Change the emulation from 2.0 to 1.1 and everything will be good to go.

[![](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-3.03.32-PM.png)](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-3.03.32-PM.png)

The default credentials are u: root / p: toor to log in. To use Metasploit for the first time there’s [some setup required](https://docs.kali.org/general-use/starting-metasploit-framework-in-kali). Using terminal, start a postgres database by running service postgressql start. Initalize the database by running msfdb init. Check Metasploit by running msfconsole.

[![](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-3.16.09-PM.png)](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-3.16.09-PM.png)

## Step 3: Setup Metasploitable2

We will need to create a linux machine and use the virtual hard drive from the .zip folder that was downloaded earlier. First step is to unzip the folder and find the Metasploitable.vmdk file.

Next go to VirtualBox and create a new 64bit ubuntu machine. Name it whatever you’ll remember. I used Metasploitable2. Click continue once everything looks correct.

[![](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-4.55.34-PM.png)](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-4.55.34-PM.png)

Change the memory size to at least 512mb and click next. There select “Use existing hard drive” and select the .vmdk file we found earlier. Last step is to click create.

[![](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-4.57.03-PM.png)](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-4.57.03-PM.png)

Start the box and confirm everything is working as expected. The default credentials are msfadmin/msfadmin. Type ifconfig to see what the boxes IP address is. This will come in handy when trying to scan for the machine from Kali. My machine is at 10.0.2.15.

[![](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-5.00.48-PM.png)](https://blog.greenjam94.me/wp-content/uploads/2017/09/Screen-Shot-2017-09-13-at-5.00.48-PM.png)

<span style="font-size: 24px; font-weight: bold;">Step 4: Double check networking</span>

Metasploitable is one of those VMs that are intentionally vulnerable for you to attack. To ensure that no one else attacks your box, make sure it can’t access the internet by confirming in VirtualBox that the network type is set to NAT.

Host-Only would work if we weren’t using another VM to use Metasploit. This is still an option if you want to install [Metasploit on your base host](https://www.rapid7.com/products/metasploit/) and skip the Kali install.

## Step 5: Attack

Now play around with Metasploit! Get on Kali, ping the Metasploitable2 machine to make sure it’s in reach. Run msfconsole for a CLI interface or open armitage for a GUI. A lot of walkthroughs are online that can be a good place to start playing with Metasploit.

## More info

For more information on how to use Metasploit, check out Offensive Security’s [free course](https://www.offensive-security.com/metasploit-unleashed/). Look for some articles such as the [series from null-byte](https://null-byte.wonderhowto.com/how-to/metasploit-basics/). Read a book about it, buy now from [No Starch Press](https://www.nostarch.com/metasploit). My motive for posting this is a lightning talk I gave at #misec this month. The [IntroToMSF slides](https://blog.greenjam94.me/wp-content/uploads/2017/09/IntroToMSF.key) are hosted here for those who are interested.