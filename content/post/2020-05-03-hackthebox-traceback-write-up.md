---
author: Greenjam94
categories:
- Community
ct_period_last_updated:
- default
date: "2020-05-03T18:06:14Z"
guid: https://www.hackerunder.dev/?p=850
id: 850
image: /wp-content/uploads/2020/05/htb_traceback_infocard.png
tags:
- walkthough
- htb
- kali
title: HackTheBox Traceback Write-up
url: /hackthebox-traceback-write-up/
---

Thanks to a zoom call with members of [PA Hackers](https://twitter.com/HackersPa). I fully exploited my first active HTB machine where I got points for my effort. To celebrate getting root, here’s my write-up. I learned quite a lot with this machine. It introduced me to new PHP web shells and message of the day (motd) privilege escalation.

## Lessons Learned

- Open Source INTelligence (OSINT) refresher with Google and Github
- PHP web shell alternatives to php-reverse-shell.php
- Privilege escalation using etc/update-motd.d

## Reconnaissance / Information Gathering

HackTheBox gives us the IP address for [Traceback](https://www.hackthebox.eu/home/machines/profile/233). It is 10.10.10.181. When I start an engagement, CTF or otherwise, I tend to run a fast port scan first and chase what I find there before wasting CPU cycles on a full system port scan.

### Port Enumeration

The first thing I did after starting the Traceback machine in my lab was run the following nmap command.

```
<pre class="wp-block-code">```
nmap -T4 -A 10.10.10.181 -oA nmap-fast
```
```

Let’s look at the options I used here. **-T4** limits the response of a TCP port to 10ms. **-A** enables service enumeration and operating system detection so we can get a better idea of what’s running on each open port. **-oA** saves three output files in different formats, nmaps standard, greppable, and XML

<figure class="wp-block-image size-large">![nmap results](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_1_nmap-1024x334.png)<figcaption>nmap shows two open ports of interest</figcaption></figure>### Service Investigation

Nmap returned two results. SSH on port 22 and an Apache web sever on port 80. We can ignore SSH, in most labs and CTFs, this is used for developing the box and reconnecting once the target is already compromised to get a full shell. Let’s look for the intended vulnerability starting by visiting http://10.10.10.181 in FireFox. While exploring the site, I ran OWASP’s [dirbuster](https://tools.kali.org/web-applications/dirbuster) to see if there were any hidden directories, but did not find any promising results.

Visiting the webpage, we’re greeted by xh4H’s message that the machine has already been compromised and a backdoor was left for us to enjoy. By inspecting the source of the webpage, we get an additional hint in an HTML comment that “the best web shells” were used.

<div class="wp-block-image"><figure class="aligncenter size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_2_index_html_comment-1024x321.png)<figcaption>inspect source of index.html at http://10.10.10.181</figcaption></figure></div>Googling for **Some of the best web shells that you might need** will lead us to a couple github repos. One Result in particular is hosted by xh4H himself.

<div class="wp-block-image"><figure class="aligncenter size-large is-resized">![Google search results as of May 3 2020](https://www.hackerunder.dev/wp-content/uploads/2020/05/Screen-Shot-2020-05-03-at-10.09.24-AM-1024x687.png)<figcaption>Google results</figcaption></figure></div>Visiting the [repository](https://github.com/Xh4H/Web-Shells) we see 16 different web shells. These are intended to be uploaded to a web server to allow information gathering, file upload, and command execution from a browser. We can start seeing if any of these shells were used on Traceback by typing the filename into the browser and hoping to get a response other than 404.

## Exploitation

We see that the second to last shell, smevk.php is on the server and we are required to login. By reviewing the [source code on the github repository](https://github.com/Xh4H/Web-Shells/blob/master/smevk.php), we see default credentials are still **admin : admin** for the user and password. Logging into that we get access to a retro-looking GUI with information about the web server. Personally I am not a fan of the color scheme (purple on black) and small text.. so first thing I did was upload a php-reverse-shell.php file from Kali to get a different shell outside of the browser.

<div class="wp-block-image"><figure class="aligncenter size-large">![smevk web shell](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_3_smevk_web_shell_upload-1-1024x415.png)<figcaption>smevk web shell, file upload is on the bottom right of the page</figcaption></figure></div>### Getting a simpler shell

Copy the php-reverse-shell from /usr/share/webshells and change the required values. Use your VPN IP address from HTB for the local IP, and port can be anything not in use currently by Kali.   
*Tip: use 443 to appear as a https web server, it won’t fool anyone but it might get you past a port based firewall. Traceback doesn’t seem to require this, but it’s a good practice to start.*

```
<pre class="wp-block-code">```
cp /usr/share/webshells/php/php-reverse-shell.php ./rev.php
nano rev.php 
```
```

There’s a few ways to upload this shell onto the webserver. While I would usually [host a local web server](https://www.hackerunder.dev/whats-updog/) with a python3 one-line command and use wget from our existing shell, this time we have an easier method. The smevk shell has a file upload feature we can use to save some time.

Setup a [netcat](https://www.sans.org/security-resources/sec560/netcat_cheat_sheet_v1.pdf) listener on Kali with **nc -nlvp 443** before visiting http://10.10.10.181/rev.php. If this is successful, you should see FireFox constantly spin where you’d normally see a favicon. In case of a “Failed to daemonize” error or other message, double check if you need to use sudo with the netcat command and that you are listening on the correct port used in rev.php.

<div class="wp-block-image"><figure class="aligncenter size-large is-resized">![PHP reverse shell on Kali](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_4_php_reverse_shell-1024x538.png)<figcaption>Got a shell as webadmin on our listener!</figcaption></figure></div>Once we get a response from Traceback, we can start further exploring the system.  
  
Don’t forget:  
– **whoami**  
– **uname -a**  
– **lsb\_release -a**  
– **cd &amp;&amp; ls -la**

###   
Persistence

Now that we have a shell, lets setup our first level of persistence. This is good because web shells can crash and regular ssh sessions offer more benefits. Most of the time, an SSH session also allows history and movement with the arrow keys where web shells are generally much more limited.

 While exploring webadmin’s home directory, we see a hidden .ssh directory. This is commonly where SSH configuration is saved. There is an authorized\_keys file. It is possible to upload our own public key and use it to ssh to Traceback as webadmin. First we’ll have to generate a new ssh key-pair and then upload the public key to Traceback. For anyone not familiar, a key-pair consists of a public and private key. Sharing your public key with servers allow them to check your private key instead of requiring a user’s password when attempting to SSH.

```
<pre class="wp-block-code">```
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub

# on webadmin rev.php shell
echo '<paste id_rsa.pub>' >> /home/webadmin/.ssh/authorized_keys
```
```

Coping the content of the id\_rsa.pub file into authorized\_keys will allow us to **ssh webadmin@10.10.10.181**. If this is successful, then kill the reverse shell and use SSH from now on.

<figure class="wp-block-image size-large">![Getting our first SSH session](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_5_ssh_as_webadmin-1024x399.png)<figcaption>Getting our first SSH session</figcaption></figure>## Privilege Escalation I – Getting User on Traceback

As we continue to explore webadmin, we should see there’s only one file in the home directory. After a quick google session, I found [gtfobins had a github page](https://gtfobins.github.io/gtfobins/lua/) on lua. By itself, I’d have many more questions… however .bash\_history connects the dots for us. We see commands that created a file called **privesc.lua** and then executed it as the sysadmin user with sudo.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_6_privesc_to_sysadmin-1024x619.png)</figure>We are able to get a elevated shell as the sysadmin user instead of webadmin. Following the same steps as with webadmin, we can add our SSH public key to sysadmin’s authorized\_keys file and create a new ssh session as sysadmin directly. For some reason, sysadmin doesn’t have the nicest shell. Once we have a shell as sysadmin, we have access to the user.txt flag and can get our HackTheBox flag.

(Anyone want to comment below or link their work suggesting how to get a full TTY shell as sysadmin at this stage of our exploitation? Will update and credit here)

## Privilege Escalation II – Getting Root on Traceback

Getting root on this box was quite a headache for me. It took me a lot of trial and error to find the intended exploit. Even once I found the vulnerable service, I could not get a shell as root. Although I was able to read single files root owned. It took me a good night’s sleep and redoing the steps slowly while being careful of syntax to realize I actually was able to get a shell.

### More information gathering

Following [G0tmi1k’s guide on linux privilege escalation](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) has gotten me past many roadblocks in previous labs and CTFs. I recommend going through the commands manually. Understanding the commands, options, and results are key to using them to find vulnerable configurations. Automation can happen after learning how things work, don’t jump the gun.  
  
The key point of information gather for me was **ps aux | grep root**. As shown below we see a command that’s running every thirty seconds to overwrite /etc/update-motd.d with a version from /var/backups. Inspecting /etc/update-motd.d we see that our user sysadmin has edit access to the files.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_8_ps_aux_root-1024x384.png)<figcaption>partial results from running ps aux | grep root, third from bottom is the script mentioned above</figcaption></figure>There’s another line from Xh4H on the motd hinting we are still on the right track.

<div class="wp-block-image"><figure class="aligncenter size-large is-resized">![](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_7_motd_research-1024x282.png)<figcaption>Traceback motd</figcaption></figure></div>### Exploiting /etc/update-motd.d

This is the first time I’ve seen update-motd.d, apparently Ubuntu has overhauled how to share the message of the day (motd)! I needed to do some googling to understand the new functionality. I read a post from [networkworld.com explaining the basics](https://www.networkworld.com/article/3219736/how-to-use-the-motd-file-to-get-linux-users-to-pay-attention.html) and a [story from lwn.net](https://lwn.net/Articles/726902/) saying how it was abused. After learning how the motd could show users unwanted ads, I looked at 00-header. It’s owned by root and these files *are bash scripts*. After learning this, I was ecstatic to see if we could get code execution as root by modifying the message of the day.

<figure class="wp-block-image size-large is-resized">![Testing exploit for motd](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_9_motd_exploit_test-1024x330.png)<figcaption>Testing exploit for motd</figcaption></figure>I appended to the 00-header script, a simple command to list the /root directory. Upon opening a new ssh session, we see “root.txt” in the message! We have command execution on Traceback and it’s possible to get the root flag for HackTheBox by simply changing ls to another command. It is important to note that every thirty seconds all changes get wiped out when the backup is applied. To maximize speed, I appended two commands together. The **-t** option sends a command to the server over SSH. **&amp;&amp; ssh sysadmin@10.10.10.181** appends a login attempt immediately afterwards.

```
<pre class="wp-block-preformatted">ssh -t sysadmin@10.10.10.181 'echo "<intended command>" >> /etc/update-motd.d/00-header' && ssh sysadmin@10.10.10.181
```

  
Getting a shell as root caused me some pain. This is where I signed off for the night, root flag in hand but no shell. My failed attempts are listed below. Hopefully these methods inspire you, I would love to get feedback if you were able to get these working or something I didn’t think of.

- Appending a passwordless root entry to /etc/passwd
- Creating a msfvenom generated payload to create a new shell
- Replacing root password
- Adding a new user with set password and adding to the sudo group
- Dumping hashes from /etc/shadow and brute forcing

### Brute force exercise

Same way we’re able to see files in the /root directory, we can view /etc/shadow and get the hashes for the root and sysadmin users.

```
<pre class="wp-block-code">```
root:$6$YZ5Z19pP$ShJsTqviOfvZo1apR2ktP198YA0U9U.KFF/yxWvZPRCZ.VkRxwZZ2DKXO3ubKsBewv8/2XzQc6YOKGofbBqEo/:18133:0:99999:7:::
sysadmin:$6$yjfs6qv1$h19rMhJv.memi/.hzPG5MAjChJHYGTO5HCUSOryF3gwzk3LpBUtYso7y9ZzZ2Rg7tDWjFuu1fE0OjVX2/9Jtj/:18133:0:99999:7:::
webadmin:$1$8UzK5eyE$Jm5DNl.IFI4ut/2K.bQUa0:18131:0:99999:7:::
```
```

Use [hash-identifier](https://tools.kali.org/password-attacks/hash-identifier) to see what the hashes format is. The shadow file uses **:** as a delimiter and the hash itself is the second value on each line. Hash-identifier told me the hashes were SHA256.

To attempt cracking passwords, I used [John the ripper](https://tools.kali.org/password-attacks/john). First I had to copy the hash to a file alone. The command I tried was **john –wordlists=/usr/share/wordlists/rockyou.txt hash.txt**. There’s a lot of passwords to attempt in rockyou.txt. It will take a long time, especially if you are on an older machine or a small VM this will hurt your performance. I was unable to find the passwords using this method, I got to the root user before this finished running.

### Getting a root shell

What eventually ended up working was making a script in the /tmp directory and executing it with the motd. The script reset sysadmin’s password to a known value and adding sysadmin to the sudo group.

```
<pre class="wp-block-code">```
#!/bin/bash

echo -e "Iamreally1337!\nIamreally1337!" | passwd sysadmin
usermod -aG sudo sysadmin

echo "sysadmin: Iamreally1337!"
```
```

*Tip: Don’t forget to make the script executable by root!* **chmod +x /tmp/resetpass.sh**

```
<pre class="wp-block-code">```
ssh -t sysadmin@10.10.10.181 'echo "./tmp/resetpass.sh" >> /etc/update-motd.d/00-header' && ssh sysadmin@10.10.10.181
```
```

After running this script, sudo su root and using **Iamreally1337!** as the password gives us an admin shell. Adding our SSH public key to root’s authorized\_keys as we did for webadmin and sysadmin will allow us to ssh directly to the box as root.

<div class="wp-block-image"><figure class="aligncenter size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_10_make_sysadmin_sudo_and_change_password-1024x364.png)</figure></div>## Clean up!

Ok, it’s a lab, just revert the box. However in practice it’s important for hackers to cover their tracks and penetration testers to revert any changes. In this case, a previous hacker (x4H4) left his signature all over the server. It’s quite the statement and the web server owner would see and react to it quickly. I prefer to remove everything except what is required for access. I’ll keep my public key on the sysadmin user as well as.

It’s important to remove anything that the public can reuse. So web shells and privilege escalation tools specifically need to be removed.

<div class="wp-block-image"><figure class="aligncenter size-large is-resized">![](https://www.hackerunder.dev/wp-content/uploads/2020/05/traceback_11_clean_up_but_keep_persistance.png)</figure></div>## Conclusion

I hope you got a lot out of this write-up. I look forward to writing more in the future and would love any feedback on this. If you have questions or problems while following along, find me on Twitter ([@Greenjam94](https://twitter.com/Greenjam94))!

## Additional References

Shout out to [0xprashant](https://twitter.com/0xprashant) for sharing [how they password protect posts](https://0xprashant.github.io/pages/decryption-instruction/) related to active HTB machines. I copied this format of setting the password in order to publish this post before Traceback retired.