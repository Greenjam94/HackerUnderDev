---
author: Greenjam94
categories:
- Uncategorized
date: "2015-12-21T18:12:03Z"
guid: http://blog.greenjam94.me/?p=227
id: 227
tags:
- blog.greenjam94.me import
title: Monitoring Honeypot Output
url: /monitoring-honeypot-output/
xyz_smap:
- "1"
---

Last week I posted in Hacking about installing a Honeypot to record SSH traffic. Since it was installed, I’ve been working on easily monitoring of the output. Michel Oosterhof, the creator of [Cowrie](https://github.com/micheloosterhof/cowrie), has done a lot of development work to create some awesome logging output from the honeypot. There are a lot of different options and you can even store output in a mySql database. I found instructions for that on [a wordpress blog](https://sehque.wordpress.com/). Feel free to checkout the current page at [greenjam94.me/honeypot.html](http://greenjam94.me/honeypot.html)

## First Attempt

After setting up my mySQL, I wrote a page of php functions to run about 8 different queries to get back information I thought was relevant. At first, the page was as simple as possible, the same page with all those functions also displayed the front end. It was a late night rushed kind of job, but you could see some pretty cool stats, in the first 24 hours of the site, we had over 500 attempts to establish a SSH connection to the honeypot. I linked the php page to my website and was happy for the night.

## Second Attempt

After looking at the page for a while, the load page took forever, it also didn’t look that good. For one thing, the code was horrible. I had so many php echo statements where there were paragraphs of unformatted HTML. The front end web page was a clear reflection of that. My goal of my second attempt was to make the webpage load fast, even on mobile devices. To accomplish that, I moved all the html to an actual html file and used Javascript and AJAX to bounce requests to the old PHP page.

### AJAX

It’s not anything new, but the site loads immediately and info fills if possible. If for some reason the PHP fails, the site doesn’t break, the page simply doesn’t display that part of the content. With the page being more formatted it was easier to make design changes. The site is slightly more responsive and mobile friendly thanks to better implementation of the Bootstrap framework.

### IP Info

The website also uses [ipinfo.io](http://ipinfo.io/) to get more information about where an IP address is located, right now the page is only displaying Country Codes, but the idea is to display the IP addresses over a map. Wouldn’t that look really cool? I want to use something like d3 but I haven’t had any experience with it so it may take a while. It’d be nice if it could look like this [cesium example](http://cesiumjs.org/d3cesium/).

### VirusTotal

Our honeypot is monitoring traffic and we see results, but we aren’t doing anything with them. Files and uploads are saved so it’s possible to analyze things further. VirusTotal is a website with a public API to review IP addresses and files for viruses and malicious code. One of my future goals is to set up a connection to the API to turn in anything that gets uploaded through the honeypot. The only issue I see is that filenames get changed, and VirusTotal doesn’t like that. The name is changed to a hash of information about the session that uploaded the file.

## Future Plans

Better use of IpInfo and VirusTotal are the next steps to improve the site. After that I’ll work on improving the site in other ways. Right now, the honeypot is logging through regular log files, JSON, and mySQL. In the five days it’s been running, we’ve had over 1,000 sessions and logging those in triplicate might get expensive on the hard drive. It’d be more efficient to write the PHP backend to pull from the JSON logs instead of needing the SQL database.