---
author: Greenjam94
categories:
- Uncategorized
date: "2015-04-19T19:25:06Z"
guid: http://blog.greenjam94.me/?p=84
id: 84
tags:
- blog.greenjam94.me import
title: Enumeration Part 1
url: /enumeration-part-1/
---

Following my post from two weeks ago about Scanning, enumeration is a Network Hackers next step. Enumeration is when you probe services (that was identified from scanning) for vulnerabilities. Now, up to this point we were able to keep a anonymous veil around us. However, enumeration requires active connections or direct queries to your target, which could be logged or capture and then used against you. Typically you are looking for usernames (that you can use for brute force guessing), email addresses (used for phishing attempts), or misconfigured/outdated systems with known vulnerabilities. The majority of information is gathered from services on FTP (port 21), Telnet (port 23), and Simple Mail Transfer Protocol or SMTP (port 25).

Banner Grabbing is hitting remote services and gathering the output for more information. Most operating systems have a [telnet](http://en.wikipedia.org/wiki/Telnet) tool you can use in terminal to access those. Try nudging to netcat to grab more information as well. Read this article all about using [netcat](https://scottlinux.com/2013/12/19/stop-using-telnet-and-start-using-netcat/).

FTP enumeration is an exploit easy access file transfer protocols. There are terminal ftp tools and even graphical ones like [Filezilla](https://filezilla-project.org/) that you can use to try to connect to a servers FTP. Servers that allow open FTP are at risk of buffer overflow which will ensure complete compromise of that system.

SMTP enumeration using the same telnet commands vrfy (verify) and expn (explain) to get info about how the servers users are set up. i.e. an example of e-mail layouts or what account is in use. [vrfy.pl](http://jeremy.kister.net/code/perl/vrfy.pl) is a tool that automates this process to make it a little easier.

DNS zone transfers are where servers dump mapping information to a remote user. This can happen if the server is misconfigured and allows any user to dump that information. DNS Cache snooping is similar, where any user can read the DNS cache and get information like frequently used URLs. An automated way to grab this information is [dnsenum](http://code.google.com/p/dnsenum) from google.

There’s a lot more about Enumeration which is why this post is only part one. There’s a lot more I want to research before trying to explain it any more. So look forward to my next post where I continue to explain Enumeration!