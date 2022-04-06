---
author: Greenjam94
categories:
- Walkthroughs
tags:
- Vulnhub
- OffSec
- Linux
- web attack
- SUID
- metasploit
ct_period_last_updated:
- default
date: "2022-04-05T19:04:27Z"
title: Vulnhub DC1 Writeup
url: /vulnhub-dc1-writeup/
---

S1ren from Offensive Security has taken on the task of regularly streaming [Vulnhub](https://www.vulnhub.com/) machines on Twitch.tv at [OffSecOfficial](https://twitch.tv/offsecofficial). She is hacking one machine a week from Vulnhub and bringing her viewers along for an interactive experience. Viewers can comment during the stream to suggest things to enumerate, exploit, or take note of as well as ask any questions. I've started to attempt the machines each week and record notes as I go. If you missed the live streams then check out OffSec's youtube page. There's a [playlist of all the  machine walkthroughs](https://youtu.be/NQ6jbKqkJ0s) hosted by S1ren and other employees.

OffSec provides these streams and videos for free publically. Vulnhub hosts these machines for free download. There's plenty of material to watch, learn, and try for yourself. My intention for these posts is to share my learning experience. I want to record failures, learning moments, and any additional resources used when exploiting the target.

# Summary

DC 1 is the first machine in the [DC series](https://www.vulnhub.com/series/dc,199/). DC machines are focused around web vulnerabilities and commonly use outdated versions of content management systems (CMS). The challenge of these machines are to exploit the web vulnerabilites and find a way ot take over the root user for each machine. DC 1 is a linux machine that uses Drupal 7 that is vulnerable to a remote code execution vulnerability. 

To start, there was no given information about the machine. Based on how the virtual networking was set up in my lab, I knew the IP address range that the machine was in. One of my [first blog posts](/posts/2015-02-13-your-hacking-environment) was about spinning up virtual machines for these kind of exercises. I had Kali and DC1 running as virtual machines on an isolated NAT network. Kali was at 172.16.250.129 and by using nmap ping scans, DC1 was found at 172.16.250.131.

On Kali, I set up a folder in the home directory for Vulnhub machines. In that folder I created another specifically for DC1. This is where I copy or download exploits to review. Information from the target machine can also be saved here, for example credentials or config files.

# Enumeration

One of the first things to do while reviewing a new machine is to scan for open ports to see what services are running. Nmap automates port scans and attempts to identify the services by banner grabbing or checking for common information.

## Nmap

To do a basic scan of DC1 I ran `nmap -sCV 172.16.250.131 -oA nmap-sCV`.

The [options](https://nmap.org/book/man-briefoptions.html) I passed with the command are listed below.

- sC and sV: run default scripts for found services and probe open ports to identiy services
- oA: create all major types of output (normal, XML, and grepable)
- host detection will happen first to check it's online
- only common TCP ports will be scanned

![nmap scan](/images/DC1/nmapResults.png)

There was only three ports found to be open.
Port 22 for SSH, specifically OpenSSH 6.0.p1 on Debian
Port 80 for HTTP or unencrypted web traffic. Specifically Apache 2.2.22 web server on Debian hosting a Durpal 7 site.
The third port was 111 for RPC bind and I did not review that service. 
Just from the nmap results a lot of information was provided for this machine.

1. It was a linux machine running Debian
2. It had 3 known services running
	- SSH for access but without creds it is a hard target
	- HTTP for a website
	- RPC
3. HTTP scripts identified a lot of additional info
	- Confirmed web server and version from headers
	- robots.txt existed
	- Drupal 7 was probably the CMS in use

## Droopescan

Since Nmap showed a Drupal website on port 80, I ran Droopescan. A tool to get specific information about Drupal instances such as identifying versions or listing installed themes and plugins.

![droopescan results](/images/DC1/droopescanResults.png)

Nmap reported the version was 7, droopescan showed it was most likely 7.22-26. Getting specific can help to narrow down exploits later that have a higher chance of being exploited successfully.

## Manual review

Visiting the IP address through a standard browser like FireFox allows us to see the website as intended.

![Target running Drupal CMS](/images/DC1/defaultDrupal.png)

Before reviewing the site, I launched burpsuite in the background to record the requests. Other tools can run in the background as while reviewing the site as well. Dirbuster, wfuzz, or vulnerability assessment tools can be ran to enumerate the website further.
There were just default Drupal pages with basic text information. Nothing that gave information about the website's purpose, users, or developers.

Nmap stated robots.txt exist. Reading that and comparing if it matched [Drupal 7's default file](https://api.drupal.org/api/drupal/robots.txt/7.x) showed there is no custom information to be learned. The admin prompt is available but we don't have credentials. If the site was not the default template we could look for identifying information and use that to brute force some login attempts.

# Vulnerability Assessment

Of the two services in nmap, only the drupal site was a clear target. A quick google search revealed that Drupal's latest version is 9.x so it is likely there is public exploits for Drupal 7.22-26
Kali has searchsploit which reviews a local version of exploit-db for possible exploits to try.

![serchsploit possibilities](/images/DC1/searchsploitResults.png)

`searchsploit drupal` gave a lot of exploits to review! I could have narrowed down my search options by specifying verion 7.22-26. However "Drupalgeddon" remote code execution (RCE) stood out to me immediately from the initial results.

There was also a metasploit option which meant I could automatically exploit the vulnerabilty after just setting a few options.

# Exploit

I tried a manual exploit first. I copied the RCE exploit into my local directory and reviewed it using nano. The exploit was written in PHP and required some files that I didn't have but maybe it could run without them.

`php 35150.php` showed some errors that proved this exploit was not viable. The warnings of missing files was expected from reviewing the code. However "Please state the cookie" and "it works only with https urls" are bad messages. The target was running HTTP unencrypted and I did not have cookie values to provide. So this exploit was not possible.

![Failed manual exploit attempt](/images/DC1/failedManualExploit.png)

While there were other exploits to attempt, I took this as a sign to spin up metasploit and get an easy win. Running `msfconsole` and searching for `drupalgeddon` showed me multiple exploits available. I used the one most relevant to Drupal 7.22-26.
The only options requried to set were the RHOSTS (target IP adress) and LPORT (local port to listen for a reverse shell on).

![metasploit exploit](/images/DC1/msfEasyMode.png)

By default, this exploit used a meterpreter payload which ensured some safe handling of the reverse shell.
Using `shell` gave a command prompt on the target machine. After some quick enumeration to check what user the shell connected as and what is on the machine, I improved the shell by adding TTY.

![access as www-data](/images/DC1/meterpreterShell.png)

www-data user is restricted and does not have permission to access many parts of the linux system. The next thing to do was find a way to get unrestricted access to the machine.

# Privilege Escalation

There's many things to check when searching for ways to get higher priveleges. G0tmi1k has a long standing blog post on [Linux Privelege Escalation](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) containing things to check on a linux host and S1ren references these in her streams.

Some of the things I checked were
- `cat /etc/passwd` users on the machine
- `ls -la /etc/*` odd services or permissions (files writeable by everyone or not owned by root)
- `ls -la /home` user's data, one existed called flag4
- `ps aux | grep root` root services
- `netstat -antup` local ports running services only for localhost

Automation can really improve the time spent enumerating for possible privelege escalation paths. It is smart to review each command first and understand the possible results, then build scripts to speed up the process. Some scripts are already public but without knowing what to look for the results will be hard to parse.

To be honest, this point was where I got stuck, not seeing access to users or any services I knew how to exploit. This is when S1ren reviewed SUID and GUID escalations. SUIDs is where calling a command as a low level user changes the active permissions to the root users. Some commands allow options that can be used to execute other commands like spawning a new shell session.
The command to search for SUIDs is `find / -perm -u=s -type f 2>/dev/null`

- find: the command we're using to search
- /: search in the base directory
- -perm -u=s: permissions for SUID
- -type f: search for files only
- 2>/dev/null: redirect errors to "null" (don't display errors)

Running find we see there are 18 programs with the permission we want to review. For new hackers, we can use [GTFObins](https://gtfobins.github.io/#+suid) to learn about each of the programs that might spawn a new root shell. With experience this will be a faster process.

![enumerate SUID options](/images/DC1/SUIDenum.png)

GTFObins shows that [find](https://gtfobins.github.io/gtfobins/find/#suid) can also be used to spawn a new root shell. Pasting from the website into our www-data shell I got multiple errors. Passing the -p parameter also made me close the www-data shell. 

![use find to get shell as root](/images/DC1/findSUIDprivesc.png)

Meterpreter did not lose connection so it was possible to reconnect quickly. After removing the -p from the find command, I had a working shell as root.

# Loot

![access as root](/images/DC1/rootProof.png)


# Lessons learned

## Target Information

OS: Debian Linux
Services:
- OpenSSH on Port 22
- Apache 2.2.22 on Port 80 hosting a Drupal 7 site

Vulnerablity: Drupalgeddon - Remote code Execution
Privelege Escalation: find SUID

## Tools and more

Droopescan is a tool that I've not very experienced with. I find it helpful to quickly indentify Drupal information.

SUIDs were a privilege escalation path that I did not fully understand. Having S1ren explain the concept and being able to play with the command helped me grasp it better. Future enumeration for privelege escalation will be easier since I know another option.
