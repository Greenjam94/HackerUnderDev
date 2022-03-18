---
author: Greenjam94
categories:
- Miscellaneous
ct_period_last_updated:
- default
date: "2020-03-03T22:10:32Z"
guid: https://www.hackerunder.dev/?p=816
id: 816
title: Patch your stuff
url: /patch-your-stuff/
---

There’s a sticker on the back of my personal laptop. I don’t recall where I got it from I believe it was an informal sticker exchange at GrrCON a few years ago. It’s a pretty clear message and you can see it [here](https://twitter.com/bagomojo/status/909816391141863424?s=20). For anyone who’s trying to watch what they say, lets call it “Patch your stuff”. It’s a simple rule, but an important one we should all follow.

## My vulnerable web server

One of the joys of having an infosec blog is that new readers are watching out for you. I shared my blog with the PA hackers slack channel and a friend asked a simple question. Which instantly raised concern and I was quick to reply.

> Are you running a honey pot on that server?

A honey pot is a system that looks intentional vulnerable and is monitored. Hackers who access it get logged and tracked. Having someone ask if I’m running a honey pot means only one thing, they were looking over the website and found something…

At first I assumed the worst. I thought someone was running malicious services or defaced the website. After a quick conversation, he sent me a picture from shodan.io. Shodan is an internet crawler that scans the public internet. It turns out I’ve been neglecting my machine, and the web server had many vulnerabilities reported for it. Most were not critical, the worst damage seemed to be it was possible to be a victim of a Denial of Service (DOS) attack. Patch your stuff, or someone else can take down your site.

<figure class="wp-block-image size-large">![Shodan vulnerability section with 7 results because my Apache web server was very outdated.](https://www.hackerunder.dev/wp-content/uploads/2020/03/Screen-Shot-2020-02-29-at-6.48.26-PM.png)<figcaption>Shodan reported many vulnerabilities for the version fo the web server I was using</figcaption></figure>The fix was harder than I originally expected. To explain, let me dox myself with some details about my setup. When I recreated this blog, I wanted to put it back on digitalocean.com instead of Github pages. I switched because the hands on experience of using a Virtual Private Server (VPS) service. To make things quick, I used their “one click install” for setting up a WordPress site on an Ubuntu server. The server itself was an older version of Ubuntu under “Long term support (LTS)”. LTS was intended to make it more stable to run the site without requiring frequent updates.

### Finding the solution

LTS was also the source of my problems. The answer to updating my vulnerable web server was not as simple as installing any updated software packages. Version 2.4.29 was as up to date as I could be, in order to get on a newer version, I’d have to update the entire operating system. It’s not that hard to do, just risky. The risk of the server failing to update or packages breaking means my “production server” could go offline and be unavailable to you. Luckily, I was able to update without any issues by using the following commands.

```
<pre class="wp-block-code">```
do-release-upgrade
apt update && apt upgrade
```
```

After getting the web server patched, it is important to confirm results. I used Shodan command line interface (CLI) to issue a new scan and prove the vulnerabilities are remediated. You can see the results yourself by visiting [Shodan](https://www.shodan.io/host/68.183.154.156). Combing through their results are free, and there are a lot of vulnerable systems out there. If you’d like to use the CLI or API, you need to buy an account, I highly recommend waiting for black Friday to get an account for a discounted price of **only $5**.

<figure class="wp-block-image size-large">![clean results with no reported vulnerabilities. This is what Shodan should look like if you patch your stuff.](https://www.hackerunder.dev/wp-content/uploads/2020/03/Screen-Shot-2020-03-01-at-8.18.54-PM.png)<figcaption>Follow up results from Shodan without any vulnerabilities reported</figcaption></figure>- - - - - -

## Getting an A on my encryption settings

After updating the server, I went to the Qualys [SSL Server Test scanner](https://www.ssllabs.com/ssltest/). This tool gives a report card grade based on the certificates used to encrypt your web traffic. Back when I had this blog at greenjam94.me, I would run this tool and modify my config as needed to get an A+ grade. It’s how I got into researching and [writing about HTTP Public Key Pinning (HPKP)](https://www.hackerunder.dev/?s=HPKP) right around the same time Troy Hunt was. Running the tool against this domain gave me a B, and I learned something pretty important: **TLS v1.0 and v1.1 are losing browser support this month, March 2020**. Still using these TLS versions locked my result to only a B.

I dug into my config files to change my settings. LetsEncrypt does all of my certificate management so I had to make changes in a few different places. My apache config under the `/etc/apache2/mod-ssl` folder and in `/etc/letsencrypt`. Both config files needed to have to following lines changed.

```
<pre class="wp-block-code">```
SSLProtocol             +TLSv1.2
SSLCipherSuite          HIGH:!SSLv3:!kRSA:!kECDH:!ADH:!DSS
```
```

After this, I also set up a CAA record. On my google domains account, I added a record that declares my encryption certificates must come from LetsEncrypt. This allows browsers or clients to check and see if there’s any unexpected certificates. Seeing a different certificate would be a sign of a man in the middle attack where someone may be intercepting the network traffic and re-encrypting it.

<figure class="wp-block-image size-large">![An "A" rating from SSL lab of Qualys](https://www.hackerunder.dev/wp-content/uploads/2020/03/Screen-Shot-2020-02-29-at-11.02.58-PM.png)<figcaption>With these two changes, Qualys was now giving me an A grade and I’m happy with that.</figcaption></figure>- - - - - -

## Conclusion

The longer a program is public, the more time there is for hackers and security researchers to find bugs in it. Vulnerabilities are discovered, new versions get released, and end users must update. If you don’t, a hacker might find your system and attempt to hack it. That’s why it’s important to regularly patch your stuff. Setup a calendar reminder and write a script to simplify the process each time. It is easier if you do it often.

## Update

When updating PHP, apt-get isn’t enough. I followed [this post](https://www.atyantik.com/working-with-multiple-versions-of-php-7-3-7-2-7-1-7-0-5-6-on-ubuntu/) and [this post](https://www.digitalocean.com/community/questions/how-to-upgrade-php-version-from-7-2-to-php-7-3-in-ubuntu-16-04) to run the following commands to switch from using PHP 7.2 to 7.3.

```
<pre class="wp-block-preformatted">```
sudo apt install php-gmagick php7.3-xml php-ssh2 php7.3-curl php7.3-gd php7.3-mbstring php7.3-mysql php-xdebug php-apcu php7.3-bcmath php7.3-bz2 php-memcache php7.3-zip
```

```
sudo update-alternatives --set php /usr/bin/php7.3
```

```
sudo apt-get install php7.3-fpm php7.3-cli php7.3-mysql php7.3-gd php7.3-imagick php7.3-recode php7.3-tidy php7.3-xmlrpc
```

```
sudo systemctl restart php7.3-fpm
```
```
sudo a2dismod php7.2
```
```
a2enmod proxy_fcgi setenvif
```
```
a2enconf php7.3-fpm
```

systemctl reload apache2
systemctl restart apache2
```