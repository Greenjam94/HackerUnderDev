---
author: Greenjam94
categories:
- Tools
date: "2015-11-25T20:33:44Z"
guid: https://hackerunder.dev/?p=186
id: 186
tags:
- owasp
title: Installing BWA (Broken Web App)
url: /installing-owasp-bwa/
xyz_smap:
- "1"
---

OWASP Broken Web App (BWA) is a safe place to practice some fun stuff and is basically a collection of applications to test everything security related. OWASP has a few projects like [Web Goat](https://www.owasp.org/index.php/Category:OWASP_WebGoat_Project), [Security Shepherd](https://owasp.org/www-project-security-shepherd/), and more. [Broken Web Apps](https://www.owasp.org/index.php/OWASP_Broken_Web_Applications_Project) is a collection of these guides and some outdated apps to test your developing skills.

## Install All The Things!

In order to set things up, it’s important to have everything you need installed. While you don’t need Kali to execute some web exploits, it is useful because of all the tools at your disposal. You do however need VirtualBox or VMware to host the VM. If you haven’t set up a VM before, I suggest you use the VirtualBox and the .osa files. The rest of this post will assume you want to use VirtualBox.

- Virtualization App 
    - [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (Controls what VMs are running)
- OWASP BWA 
    - [For VirtualBox](http://sourceforge.net/projects/owaspbwa/files/1.2/OWASP_Broken_Web_Apps_VM_1.2.ova/download) ( .osa open virtualization application)
- [Kali 2017.1 VM files](https://images.offensive-security.com/virtual-images/Kali-Linux-2017.1-vbox-amd64.ova) (Not needed, but fun tools)

## Extract Kali Files

Mac users can go to the app store and download The Unarchiver, a free app to extract 7zip files. Windows can download the [7-zip program](http://www.7-zip.org/download.html) and extract using that. Gentoo users can use p7zip, a command line 7zip tool. It’s apparently in the debian repos. Extract that with the command `7z x archive.7z`.

## Setting it up (For VirtualBox)

Once everything is installed, run VirtualBox, and click File > Import Application. You will choose the files that you just downloaded from the links above. The default settings that come from the app should appear and you can click ok.

After you see a new Virtual Machine available, you’ll want to check and make sure it you can access it from either your Kali VM or your actual machine. To do this you can set up either a NAT network or a Host-only Adapter for the VM. I choose to do Host-only but either will work as long as you configure it correctly. Go to File &gt; Preferences. On the left sidebar click Network. There you’ll see tabs for NAT and Host-only. Pick one, there will be three buttons on the right, click the one with the + icon. That will add a new network for your VM to use. Hit OK to go back to the main page of the app.

Go to your VM’s setting by clicking to select it, then hit the big settings button above it. Go to Networks, then select the network type you just made and be sure the network is correct then hit ok for everything.

## Start up the BWA

Run the VM! This should go without a hitch, the VM should start up in a new window, as it does it should display a lot of lines as it’s starting but once it’s ready to log in you can use the credentials root:owaspbwa. This is the default user and password for all the administrative accounts across the VM. As you log in, the entrance text should say what the IP address is for the VM. On your actual machine, open a web browser and try to go to that site by typing the ip address into the URL.

To make it easier for future use, you can edit the hosts file on your main computer to accept a url instead of an IP address. On linux or a mac, open a terminal and write `sudo nano /etc/hosts` and add a line to the end of the file including the IP address and the URL to assign. Use a tab between the IP Address and the url. The URL can be whatever you want if you don’t like owaspbwa.com. Change the IP Address in the browser and replace it with the new url. If that doesn’t work, try restarting the browser and try again.
