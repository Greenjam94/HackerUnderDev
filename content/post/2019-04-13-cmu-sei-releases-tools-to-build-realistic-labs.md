---
author: Greenjam94
categories:
- Miscellaneous
ct_period_last_updated:
- default
date: "2019-04-13T16:57:10Z"
guid: https://www.hackerunder.dev/?p=774
id: 774
title: CMU SEI releases tools to build realistic labs
url: /cmu-sei-releases-tools-to-build-realistic-labs/
---

In order to learn something, you need to practice it. When it comes to becoming a hacker, that is done by attacking machines in a lab. There are many ways of doing this, such as building your own, spinning up an [OWASP](https://www.owasp.org/index.php/OWASP_Broken_Web_Applications_Project) or [Metasploitable](https://www.hackerunder.dev/installing-kali-metasploitable-virtualbox/) virtual machine, or using a service like [Hack The Box](https://www.hackthebox.eu/). There’s one common flaw with these labs though, they’re not realistic. To build realistic labs that look like live environments is a hard task to accomplish. The [Software Engineering Institute of CMU (SEI) has open sourced 6 tools](https://www.sei.cmu.edu/news-events/news/article.cfm?assetId=540280) that they have been using to provide realistic training to clients for years.

<figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/v0Y9xQG55W4?feature=oembed" width="500"></iframe></div></figure>**Disclaimer:** These tools are all provided by the Software Engineering Institute of CMU. I attended their webinar linked below and am interested in starting to use these tools. I do not own or contribute to these tools in any way. This post and a related talk I gave is a summarization of my notes from the webinar, for more information please check out SEI’s posts linked throughout the article.

When referring to a lab, I will use two terms to describe the people involved. A *player* is a someone who’s using a lab intending to learn new skills. A *builder* is someone hosting the lab to provide the learning experience. In some cases, like a home built lab, a hacker can be both roles.

## Ghosts

One of the major differences between a lab environment and a real one is the activity. A real environment has users that use applications, click links, and does business. Users provide an attack path for social engineering, client side attacks, and more. These techniques are hard to replicate in a lab unless a builder or player acts as a user. [Ghosts](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=539699) is a framework to provide realistic user activity in a lab. This activity comes from Non Playable Characters (NPCs) like a video game. These NPCs are able to do a wide variety of activity such as web browsing, terminal commands, email, and using office documents. The framework is also able to conceal the administration side of controlling the NPCS, as well as reporting on network health and malicious code status.

## TopGen

One of the biggest concerns of hosting a lab is keeping it safe. Separating the labs from the internet is a good measure to do this, it prevents users from mistaking real websites for targets and it prevents external entities from interfering with the lab. When this happens, it really limits the realistic aspects of a lab because it limits what’s possible. Imagine how much work you do daily that involves google, amazon, or other services that are over the internet. To improve this, SEI created [TopGen](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=539707) to simulate the frontend of the internet. TopGen servers multiple co-hosted virtual services such as HTTP vhosts, DNS, and mail domains. By scraping a configured list of websites and generating certificate authorities and self signed certs, it becomes possible to recreate the internet inside the lab.

## GreyBox

It’s one thing to scrape the front end of popular websites to recreate the internet realistically. SEI has taken it a step further to recreate the infrastructure of the internet as well. [GreyBox](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=539705) simulates the backbone of the internet by emulating DNS servers, Internet Service Providers, and other infrastructure components.

## TopoMojo

Another constraint of labs besides realism is resource management. Labs have drastically smaller resources available than real environments. One way SEI attacks this problem is a custom management interface to spin up lab machines only when requested by players. Machines are only online when they’re needed, allowing more responsible use of limited lab resources. [TopoMojo](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=539709) also allows players to become builders by providing templates for machine customization and expansion.

## vTunnel

Another factor that prevents realism is the need for lab builders to control and maintain machines and events during the lab. Usually this is done over SSH or services that have to be marked as do not use. For example, a scoreboard tracking flag submissions, usually this is not allowed to be the target of players. SEI built [vTunnel](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=539703) to provide a separate communication channel that hides these administrative functions from the lab. VTunnel uses a tunnel through the hypervisor of the lab to separate traffic from the lab environment. This reduces the chances of players getting knowledge from builder activity instead of the intended path.

## WelleD

All of the tools mentioned so far are great for labs commonly seen in CTF environments where the target is virtual machines emulating client hosts and servers. These labs and CTFs are great for attacking applications and web services. Another attack path that has been hard to recreate in a lab is wireless attacks. We commonly hear about man in the middle attacks and sniffing our web traffic on public networks. Usually, to create a lab to test this, multiple wifi routers are required and this fills the air with real radio waves. SEI has found a way to get around this by perfectly emulating 802.11 wireless frames using [WelleD](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=539701). This creates virtual traffic using real protocol standards, all of the wireless tools used today will work as if this lab traffic was real wireless communications.

## Conclusion

The Software Engineering Institute has open sourced 6 tools that will greatly improve the realism of labs. They provide labs the ability to simulate clients, the internet, and wireless communications as well as providing a means to save computing resources and hide administrative actions. I provided a [lightning talk at misec](https://www.youtube.com/watch?v=1Xop0orz6K4) sharing this same information with the local Michigan infosec community. I hope this introduction to SEI’s tools inspire you to check them out and build realistic labs.