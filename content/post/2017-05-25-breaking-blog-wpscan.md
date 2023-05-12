---
author: Greenjam94
categories:
- Best Practices
date: "2017-05-25T17:49:12Z"
guid: https://hackerunder.dev/?p=596
id: 596
tags:
- vulnerability_analysis

title: Breaking My Blog with WPscan
url: /breaking-blog-wpscan/
---

One of the [tools offered by default in Kali](http://tools.kali.org/web-applications/wpscan) and many other hacking related distros is WPscan, a black box WordPress [vulnerability scanner.](https://wpscan.org/) I wanted to learn how to use this tool because it would help with recon on CTF challenges, practice boxes from [vulnhub](https://www.vulnhub.com/), and even trying to keep my own blog vulnerability free.

## Disclaimer

Before I tell you more about the tool and how it can be used, I have to throw out the usual disclaimer. Do not use this tool on any WordPress site you don’t own or have permission to scan. I ran WPscan on my own blog and brought the whole site to it’s knees. If you did that to a professional blog or your company’s blog then you could have major problems when you get caught.

## Setup

If you don’t have a personal WordPress instance to test, then look at getting [OWASP’s Broken Web App installed](https://hackerunder.dev/installing-owasp-bwa/). It has a vulnerable instance of WordPress included that you can scan against and even try to harden. When things break, just reinstall the VM and start over from scratch.

I have a Kali VM installed on most of my computers. By opening a terminal and typing **wpscan -h** you can learn about all of the options and possibilities for how to scan a WordPress instance. If you want to avoid all the “bells and whistles” then just run **wpscan –url &lt;target&gt; –enumerate p** to scan and try to find vulnerable plugins.

## The scan and the results

WPscan scans the site looking for version numbers and other exposed information that can be compared to a database of vulnerabilities. While running the scan against my site, there was one vulnerability found. There was also a small issue because the WordPress version was public. I was able to research the vulnerability using the links provided from the scan output. While the “fix” isn’t a perfect patch, it’s better than ignoring the vulnerability until the next update is ready. The version was easy enough to hide by deleting the stock readme.html file.

The issue wasn’t removed on the next scan. The scanner is a quick tool to find some vulnerabilities based on version numbers and other information from the site. There’s no proof of concepts or attempts to validate if a vulnerability is a false positive.

## Unexpected fun with WPScan

While running the scanner the **–enumerate p** option will brute force the website with requests to gather information about plugins. This crippled my site when over 20 simultaneous database connections made the site unresponsive. This wasn’t solely due to the scanner, but to the fact that I created this blog in college. Which means I took the cheap and easy route. Digital Ocean’s one click solution on the smallest VM made that possible.

The reason I’m not afraid to tell all you hackers about this is because I was able to resolve the issue with some help from friends at [\#misec](http://misec.us/). They told me about [adding a swapfile](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04) that could help boost some power to my little VM. Since then I’ve been able to run WPscan multiple times without a single fatal performance issue.

Edit: A swapfile is NOT recommended for preventing DoS resulting from scans or a spike in traffic. This is a quick fix and a cheap band aid. If your company uses WordPress or you have a personal blog thats getting a lot of traffic, upgrade the VM or migrate to a better server that can handle the requirements. This was pointed out to me when I first heard of using a swapfile and again on Twitter after posting this.

<figure aria-describedby="caption-attachment-601" class="wp-caption aligncenter" id="attachment_601" style="width: 300px">[![](https://hackerunder.dev/wp-content/uploads/2017/05/Screen-Shot-2017-06-05-at-9.26.06-PM-300x74.png)](https://hackerunder.dev/wp-content/uploads/2017/05/Screen-Shot-2017-06-05-at-9.26.06-PM.png)<figcaption class="wp-caption-text" id="caption-attachment-601">Twitter reply</figcaption></figure>