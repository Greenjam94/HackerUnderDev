---
author: Greenjam94
categories:
- Exploitation
ct_period_last_updated:
- default
date: "2020-02-26T02:18:42Z"
guid: https://www.hackerunder.dev/?p=802
id: 802
image: /wp-content/uploads/2020/02/updog_github.png
tags:
- kali
- python3
- web server
title: What is Updog?
url: /whats-updog/
---

Friends among my various hacker spaces have shared links to a new tool called *[Updog](https://github.com/sc0tfree/updog)* created by [Sc0tfree](https://twitter.com/sc0tfree). A python3 implementation of an HTTP server that is intended to replace Python2’s SimpleHTTPServer module. I had to test it out myself and these are my opinions. In many hacker training courses, it is vital to be able to host your tools on a web server to download them onto target machines. One example would be to download a network scanning tool once you’ve gained a shell on the first machine of a target network. With Python2, this was done with a one line command `python -m SimpleHTTPServer 8000`. Let’s take a look at some of the options now that Python2 is finally being sunset.

## Installation

To get started, I set up a brand new virtual machine. I downloaded the kali 2020.1 VirtualBox image from [Offensive Security’s website](https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/). One big change with 2020.1 is a default non-root user, so to log in you will need the new standard credentials. Instead of `root`:`toor`, now login with `kali`:`kali`. `toor` is still the password used by the root user.

```
<pre class="wp-block-preformatted">sudo apt update<br></br>sudo apt install -y python3-pip<br></br>pip3 install updog<br></br>export PATH=$PATH:/home/kali/.local/bin
```

Once Kali is running, it is time to install updog. The README on github mentions using pip3, so get that done first. After pip3 installs updog, the output gives a warning of the tool not being on PATH. You can add to the environment variable for a permanent fix.

## Usage

Updog is super simple, and most importantly *short*, to run. By default you can `cd` to the directory with the files you want hosted and run `updog`. This will start a local web server on port 8080.  
  
There’s a few options you can play with as well. Setting a custom port is easily done with the `-p` option, HTTP basic auth can be used with `--password`, and encryption using SSL is possible with `--ssl`.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/02/updog-1024x640.png)<figcaption>Running updog on Kali Linux 2020.1 and visiting the resulting web server</figcaption></figure>## Comparison

So why use Updog? To answer this, I’m going to do an apples to apples comparison between Updog and the python3 variant of the SimpleHTTPserver one liner: `python3 -m http.server 8000`

**Usability: +0** In a pinch, whatever is faster to type I’ll use first. However http.server doesn’t require setup  
**Appearance: +1** Updog offers a superior well-designed interface for access. If manually downloading files and visually accessing your machine from a browser, Updog looks a lot better than the default directory index you’d see with http.server  
**Network Volume: -1** The graphics of updog require a few static files to create, not uncommon when visiting websites, it is considerably noisier on a network than a single request to download a file.  
**Security: +2** Updog offers encryption. Using a self-signed SSL certificate prevents network traffic from being readable. Setting up a password can also prevent any other users who find your web server from downloading your tools and payloads.

Below you can see a visual comparison of both web servers, the command required to run them, and the resulting network traffic of just visiting the hosted directory.

<figure class="wp-block-gallery columns-2 is-cropped">- <figure>![](https://www.hackerunder.dev/wp-content/uploads/2020/02/python3-1024x640.png)</figure>
- <figure>![](https://www.hackerunder.dev/wp-content/uploads/2020/02/updogSSL-1024x640.png)</figure>

<figcaption class="blocks-gallery-caption">Side by side comparison of http.server one liner and updog –ssl on Kali Linux 2020.1</figcaption></figure>## Conclusion

Overall, I’d recommend trying Updog on your next hacking engagement or CTF. Just keep `python3 -m http.server` in your back pocket. No single tool is perfect for every engagement. It’s good to have multiple ways to move files to a target machine. Both of these tools use python. For even more options to save in your toolkit, [checkout this gist](https://gist.github.com/willurd/5720255) with all the web servers variations in multiple web languages.