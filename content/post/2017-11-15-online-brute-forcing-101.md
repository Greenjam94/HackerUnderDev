---
author: Greenjam94
categories:
- Best Practices
date: "2017-11-15T17:51:14Z"
guid: https://hackerunder.dev/?p=646
id: 646
tags:
- Exploitation

title: Online Brute Forcing 101
url: /online-brute-forcing-101/
---

A good friend once mentioned how cool it’d be to practice brute forcing for a website login. I created a simple web page with a login form. Incorrect logins display a red error message while successful logins show the rest of the web page. There’s no database or complex code behind the webpage. It simply hashes the user input and compares it to a stored value.

Before we continue, I must make it blatantly obvious that **hacking any online service without consent could land you in a lot of trouble**. For your safety, do not hack any systems outside of your personal property or online labs / tutorials that give consent.

There is a couple tools that will help with brute forcing an online form. Two tools that I will demonstrate in this post is Burp suite’s intruder module and Hydra. Both tools are available on Kali linux.

## Burp Suite

Burp suite is a proxy from [Portswigger](https://portswigger.net/). My example website used HTTPS and did not allow unencrypted traffic, you’ll have to [install and trust the Portswigger certificate](https://portswigger.net/burp/help/proxy_options_installingcacert) which is generated by the proxy.

On the left hand side of BurpSuite Targets page you can see all the files being called. The login form and the passwords text file are caught by the proxy. Every POST attempt to login will be recorded. The image above shows one attempt.

Right click a POST request and click “Send to Intruder” or highlight it and press CTRL+I. This will take you to the Intruder module. Go to the positions tab. We do not want to change the user value, so remove that. The page should look like the following.

Next go to the payloads tab. Either paste the values in from the passwords text file or upload the entire file if you saved a local copy. There should be 23 requests.

Click the “start attack” button in the upper right corner. The attempts should go by quickly, even if using the free version of Burp suite. The error messages are not returned as HTTP codes, every response is 200 (OK). Our saving grace is that the successful login shows more content, so the size is larger. Maybe the columns of those results are orderable?

## Hydra

Hydra is a command line tool that can make quick work of many kinds of brute force attempts. However the syntax can be a little confusing. Below is the command to use in order to bruteforce this form. You’ll also have to save the passwords file locally before trying this.

**hydra -l ‘iAm’ -P wordlist.txt 127.0.0.1 https-post-form -I “/login.php:name=^USER^&amp;pswd=^PASS^:Incorrect password”**

I will break down the options used above.

- -l username value, (a lowercase L)
- -P password file, uppercase options are used for files with multiple values to attempt
- 127.0.0.1 IP, the next requirement is the target to attack, either IP or domain works
- https-post-form form type, this is the kind of form you’re targeting a full list is provided in the man pages (manual)
- -I a option to prevent trying to load restore files on multiple attempts to run hydra
- “1:2:3” form parameters separated by a colon, 
    - 1 is the file after the domain which always starts with a “/”
    - 2 is the input names and values. ^USER^ and ^PASS^ must be used
    - 3 is an expected error message, it doesn’t have to be the complete message. (Don’t use colons unless escaped)

Feel free to try each method out once on your own on my login page! Please don’t do it repeatedly though, this is basically asking everyone to DOS me and I want to keep my websites up without pissing off my hosting company.

After finding the correct password, I suggest typing in the password manually and visiting the successful web page.

Know of any other good tools for online brute forcing? Let me know in a email or on twitter. I hope you found this post to be helpful.