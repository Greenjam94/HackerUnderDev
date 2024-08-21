---
author: Greenjam94
categories:
- Community
tags:
- walkthrough
- offsec
- provinggrounds
ct_period_last_updated:
- default
date: "2024-08-20T00:00:00Z"
title: Proving Grounds Scrutiny Writeup
url: /proving-grounds-scrutiny-writeup/
---


Full disclosure: This is my notes from watching Plantplants, a student mentor at OffSec, on a [Twitch](https://www.twitch.tv/offsecofficial) live stream. The video will be reposted to OffSec's youtube soon.

## Setup

After launching the targe machine on the OffSec portal, set a local zsh environment variable to the target IP `export IP=10.0.0.2`
The variable can then be used in future commands.

## Recon

Plantplants mentioned he liked to do manual enumeration while the all port scan ran in the background. It's not an uncommon idea, a lot of targets have similar themes in labs like this, websites on port 80/443 or FTP and mail services. Personally, I would suggest a quick nmap scan to check default ports to take out the guess work before manually looking deeper into services.

full nmap command `nmap -p- -A -T4 -Pn $IP -oN nmap_result.txt`
Nmap options briefly explained: https://nmap.org/book/man-briefoptions.html
- All ports -p-
- OS detection, version, scripts and traceroute -A
- faster timing template -T4
- Treat as online, skip host discovery -Pn
- normal output saved to given file -oN filename

Manual enumeration was focused on the website, looking for some common points of interest like pages available, frameworks used, login portals, or comments in the HTML source code.
Another lab tip, if the links on a webpage point to a domain, add a local rule to your /etc/hosts file that points the domain to the lab target's IP address. This way any exploits or path resolution dependent on DNS could still work.

Plantplants found a subdomain with a login prompt for TeamCity https://www.jetbrains.com/teamcity/ that displayed the version number in use.

## Vulnerability research

With a known software and target version, the next goal is to research vulnerabilities for that software. Google dorking https://www.offsec.com/blog/exploit-db-2022-update/ and searching for the software in exploit-db.com is a good initial step. Plantplants searched for the JetBrains TeamCity website and found their [fixed security issues](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity) page and was able to search for additional CVE information on cvedetails.com

After finding [CVE-2024-27198](https://www.cvedetails.com/cve/CVE-2024-27198/) and that public exploits exist the next step is to find said exploit code. There were no entries in exploit-db but github is a great secondary option. [https://github.com/W01fh4cker/CVE-2024-27198-RCE](https://github.com/W01fh4cker/CVE-2024-27198-RCE) is a public python based exploit and Plantplants decided to attempt exploiting it on stream. The README in the Github repo advertises that it creates a TeamCity user and attempts to create a RCE shell.

I would like to add a caveat that it is wise to know the exploit script well before running it. Some Github repos could host fake exploits that abuse your system when it runs. Read the code so you have at least an idea of whats being attempted. If there's any large chunks of obfuscated or encoded text in the script there is a good chance that the script is dangerous to run. You could also make improvements, one example is changing the username and password parameters for the created user. Setting a specific username may be more helpful in a realistic engagement then having random characters.

## Exploitation

Running the python3 script was able to create a username and password to log into TeamCity. Exploring the application revealed a commit history where a user uploaded their id_rsa file for ssh connections.

Copying the id_rsa file to Plantplant's attacker machine and attempting SSH revealed that a password was still required. This is where password cracking comes into play with JohnTheRipper. Cracking revealed the password.

## Privilege escalation

Now that initial access to the system was found. More enumeration is required. The connection message revealed there was mail available so that was the first place to look. Sometimes there was other common places to review like the user's bash history, file permissions or sudo permissions. The user's mail revealed a second user's name and password.

As the second user, enumeration on the target system continued. Reviewing the new user's home directory revealed a third user's name and password in a file.

The third user had a systemctl command with sudo rights, running that command allowed the user to execute commands as root by spawning a sh shell. For more info on using systemctl for privesc, see the [systemctl section on gtfobins](https://gtfobins.github.io/gtfobins/systemctl/).

## Key take aways
- understand nmap parameters and start with an enumeration
- manual review of services can be just as helpful to understand target services
- google is one of the best hacking tools to find reported vulneraiblities and public exploits
- Review public exploit code before running anything
- Repeat enumeration with each level of access to find additional information
- User error and misconfigurations can be just as critical as known exploits
