---
author: Greenjam94
categories:
- Uncategorized
date: "2015-12-18T15:55:51Z"
guid: https://hackerunder.dev/?p=224
id: 224
tags:
- blog.greenjam94.me import
title: Cowrie Honeypot Installation
url: /cowrie-honeypot-install/
xyz_smap:
- "1"
---

Who likes honey? I know I do. Unfortunately Cowrie isn’t the like of honeypot you might imagine. Instead of thinking source of deliciousness, think something y![Winnie the pooh](https://hackerunder.dev/wp-content/uploads/2015/12/PoohHoneypot-300x224.gif)ou will get your hand stuck in. In security terms a [Honeypot](https://www.sans.org/security-resources/idfaq/honeypot3.php) is where a system is set up to record everything that’s going on. In those terms, cowrie is a SSH monitor that tracks everything that happens over an ssh connection.

This is a project that I started with [@Taco\_Pirate](https://twitter.com/taco_pirate). He had a digital ocean VM that he wasn’t using and we decided to turn that into a honeypot of some sort. At first we were going to use Kippo, but after reading though some of the wiki we found that Cowrie is just newer and more updated flavor of Kippo.

So what did we end up doing to get everything set up? Well first we started with a basic linux installation on the VM. From there we followed the instructions on Kippo to install any necessary services.

```
sudo apt-get install build-essential python-dev libmysqlclient-dev python-virtualenv python-pip
```

It’s probably also a good idea to run apt-get update first as well. From there we followed the instruction’s on cowrie’s [INSTALL.md](https://github.com/micheloosterhof/cowrie/blob/master/INSTALL.md). First we created a user, cloned the directory from the [github repo](https://github.com/micheloosterhof/cowrie), and then copied the configuration. By default, the honeypot listens to port 2222, so we set up an IP table config rule to port any traffic from port 22 to go there. That rule is:

```
sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222

```

For admin reasons we also opened another port for Taco\_Pirate and I to connect to. All it takes to start the honeypot is a single command from the directory that Cowrie is installed to which is “./start.sh” \\, stopping is as simple as running “./stop.sh”. It was a pretty easy set up, so this post is short compared to my last few but that’s good. If you’re interested in learning about how people or robots (scripts that do malicious things) interact with servers over SSH or what people use to try and guess accounts or passwords, I would suggest you make a honeypot for yourself.

The author of Cowrie started with kippo and made some really cool additions, you can read them all on his [website](http://www.micheloosterhof.com/kippo-modifications/). Some of them that I really like are the SFTP support and improved logging. There are a lot of options in the config file for different uses of the logs. For example you can add attempts to a MySQL database or send files to [VirusTotal](https://www.virustotal.com/) in order to see if things are malicious. Integrating with VirusTotal was something I requested from the creator of Cowrie and he set it up really quickly. If you have a similar idea, you can create an issue on [Github](https://github.com/micheloosterhof/cowrie/issues).

Thanks for reading this, if you have any questions about how I implemented Cowrie then please comment below. If you want to view the statistics of the people getting caught then feel free to look [here](https://greenjam94.me/honeypot.html).