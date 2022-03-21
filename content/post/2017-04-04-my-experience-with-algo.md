---
author: Greenjam94
categories:
- Uncategorized
date: "2017-04-04T23:33:55Z"
guid: https://hackerunder.dev/?p=545
id: 545
tags:
- blog.greenjam94.me import
title: My experience setting up an Algo VPN
url: /my-experience-with-algo/
---

First off, I don’t know if you’ve been avoiding the political storm as much as I have but there’s one thing that’s been so retweeted, shared, and updated that I couldn’t avoid it. The discussion about the privacy of our internet content.

## The Problem

[ISPs are able to sell your data](https://www.usatoday.com/story/tech/news/2017/04/04/isps-can-now-collect-and-sell-your-data-what-know-internet-privacy/100015356/). While it is possible that similar data is already being collected and used by social media, applications, and other providers… It’s brought up an interesting conversation about how to secure ourselves while browsing the internet.

Using [TLS](https://hackerunder.dev/tls-what-is-it-and-why-it-matters/) to encrypting the communication between a client and server is a good way to secure the content between you and a website. However what about your destination, ip address, and other information that’s required to connect to that server? Virtual private networks (VPNs) have been used by corporations and security focused individuals for years. Lately VPNs are the center of attention because they offer a way to encrypt information about your host and prevent your real location from being collected. To learn more about what a VPN is, check out <https://www.bestvpn.com/blog/38176/vpns-beginners-need-know/>.

## A Solution

I’ve been toying with the idea of using a VPN for a while now. Going to security conventions and using the hotel’s public wifi has never let me sleep well at night. A VPN would minimize that issue. I’ve considered a few paid services but ultimately decided to go for the “create your own for cheap” route. The infosec community has been buzzing about Algo. [Algo VPN](https://github.com/trailofbits/algo) “is a set of Ansible scripts that simplify the setup of a personal IPSEC VPN. It uses the most secure defaults available, works with common cloud providers, and does not require client software on most devices. See our [release announcement](https://blog.trailofbits.com/2016/12/12/meet-algo-the-vpn-that-works/) for more information.”

This blog is hosted on a DigitalOcean droplet. I’m familiar with how droplets work and when I heard that Algo can create a droplet and use it as a VPN provider, I jumped at the opportunity.

## How I setup my Algo VPN

Following the [README.md](https://github.com/trailofbits/algo) of the Algo github repo is very straightforward. The idea is to clone the repo to your local computer. After installing the dependencies and setting up the config file for the number of users to expect, Algo takes care of all the heavy lifting by using DigitalOcean’s APIs to create the droplet and setup the VPN.

I cloned the repo onto my mac, installed the python dependencies and only had one [hiccup](https://github.com/trailofbits/algo/issues/351). On a mac, you need to have Xcode installed and agree to the license. All of the files required to setup the VPN clients are saved to the config folder after running the script. To connect my mac, all I had to do was double click the &lt;username&gt;.mobileconfig file and everything was fully setup.

[![](https://hackerunder.dev/wp-content/uploads/2017/04/Screen-Shot-2017-04-04-at-10.43.10-PM-300x109.png)](https://hackerunder.dev/wp-content/uploads/2017/04/Screen-Shot-2017-04-04-at-10.43.10-PM.png)

I’ll have to update this post as I setup my other devices. Windows computers and Android phones are on my to do list.

To test if the VPN is working, visit [whoer.net](http://www.whoer.net). Check to see if the host connecting to the site is the droplet IP or your computer’s IP. The caveat of using such a VPN is that it’s not fully anonymous. Website hosts can know your connection is coming from a DigitalOcean droplet because who owns the IP range is publicly available. Similar to the risk of someone [watching a Tor node](https://nakedsecurity.sophos.com/2015/06/25/can-you-trust-tors-exit-nodes/), well known VPN providers can also be monitored. It is only a matter of time before the usage of that droplet is mapped out.

## That’s not all folks

VPNs are only one part of a secure digital life. Using HTTPS when connecting to websites, resetting passwords every few months, and enabling two factor authentication is also important. As far as “providers selling our data”… The best way to prevent that is to choose providers with a stronger commitment to their users than those who care more about improving their profits.