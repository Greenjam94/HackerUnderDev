---
author: Greenjam94
categories:
- Just for fun
ct_period_last_updated:
- default
date: "2020-04-30T00:37:51Z"
guid: https://www.hackerunder.dev/?p=826
id: 826
tags:
- gaming
- minecraft
title: Starting a Minecraft server
url: /starting-a-minecraft-server/
---

I’ve had a love hate relationship with Minecraft since I bought the beta when I was in highschool 2011. Throughout the years I’ve played countless local games, and joined others online in public servers. Over the last decade, I cycled through playing, getting mad at myself for not being productive, and taking a break. Lately I’ve been watching [Hermitcraft](https://hermitcraft.com/) more than playing or working combined. To convince myself I was being productive, I told myself starting a Minecraft server to play with friends and family would be a worthwhile **systems administration** and **security** project. Not just an excuse to play games for days on end without sleeping.

## Create the server

Originally, I was using Digital Ocean to host the Minecraft server. A $10/mo virtual private server provides enough RAM to play the game with a few friends. After a couple months of using it, I decided $10/mo was too much for Minecraft and I setup Ubuntu server on an old gaming desktop I don’t use anymore. The system you use for a server doesn’t really matter, as long as it can run Java it should work. Using a hosted virtual server though cuts out some of the networking work required if you use a machine on your home network.  
  
A note about networking. If you’re playing with people on the same network as your server, you don’t have to worry about routing or setting up a domain to use. However if you’re trying to get others to connect to your server over the internet you’ll need to do two things. First, set up port forwarding on your router to forward traffic on port 25565 to your Minecraft server. Secondly, use a service like noip.com to create a domain that points to your home router’s IP address. Your router might have reverse DNS configuration that will update noip if the IP address ever changes. Now you’ll be able to give your noip domain to your friends.  
  
If you’re recreating this, take note of some required specs. At a minimum for 1-4 players, you’ll need 1 GB of ram and a few hundred MB of storage space, you can accomplish this on a cheap RaspberryPi.  
However just exploring around the world causes some serious lag. I’d recommend 4GB of RAM, the wiki recommends that and I’d consider it as a minimum. More could always be better. My custom built PC has 32 GB of ram and a half terabyte of storage, way more space than I need for a Minecraft server… but at least it’s getting some use now!

## Set up the server

There’s a few steps to follow initially, most are good practices that secure your instance.

First, add a new user on the system. Minecraft doesn’t need to run as root so create a low privilege user instead.

```
<pre class="wp-block-preformatted">adduser minecraft
```

  
Secondly, in order to run the game, you’ll need to install java on the system. Once that’s done the next step is to download the game. These steps I found on [https://minecraft.gamepedia.com/Tutorials/Setting\_up\_a\_server#Debian\_and\_Ubuntu](https://minecraft.gamepedia.com/Tutorials/Setting_up_a_server#Debian_and_Ubuntu)

```
<pre class="wp-block-preformatted">sudo apt-get install openjdk-8-jdk-headless

# visit https://www.minecraft.net/en-us/download/server and copy the link to download the server.jar file
wget <paste_link_from_minecraft> -O server.jar
```

  
Next it’s important to setup a firewall. At this point you shouldn’t have many services running, however it’s safe to set this up anyways. Personally my ufw firewall blocks SSH from any out of network computer and only allows traffic on the minecraft port.  
  
screen is another key program that allows you to run a program in the background without losing your current SSH shell.

## Starting the server for the first time

It’s important to run the game manually first, to accept the EULA and configure the game. On first run, important files will be generated. Once that’s done, you can edit server.properties to customize your game.

```
<pre class="wp-block-preformatted">java -Xms1G -Xmx1G -jar server.jar

java -Xms4g -Xmx16g -jar minecraft_server.1.15.2.jar nogui
```

Notice the parameters. By default, most servers need 1 gigabyte to run. As I said earlier, my machine is larger so I give it much more than required to get smoother gameplay.

## Automate the boring stuff

I setup a script on a cron job to restart the server daily, this automatically keeps the server running well without processes getting out of hand for running continuously for days on end.  
  
I’ll explain how the script works, feel free to copy and paste the code below.  
First, it changes directory to where Minecraft is currently running and creates a log entry. If there’s currently a screen session running Minecraft, it will broadcast a message in game warning the server will be restarted. After waiting, the session is ended and a new one is created in its place. Another log entry is created to mark the script executed successfully.

```
<pre class="wp-block-preformatted">#!/bin/sh

cd /home/minecraft/1.15
echo $(date -u) " | Starting server auto_run script attempt" >> auto_run.log

if screen -list | grep -q "minecraft"; then
    echo $(date -u) " | Found running screen session named 'minecraft'" >> auto_run.log
    screen -S minecraft -p 0 -X stuff "/say SERVER RESTARTING IN 5 minutes!^M"
    sleep 5m
    screen -S minecraft -p 0 -X stuff "/say SERVER RESTARTING NOW!!^M"
    sleep 30s
    screen -X -S minecraft quit
fi
sleep 2m
screen -S minecraft -d -m java -Xms4g -Xmx16g -jar minecraft_server.1.15.2.jar nogui


echo $(date -u) " | Finish server auto_run script" >> auto_run.log
```

## What makes it secure?

Running as a low privilege user prevents a hacker from abusing the exposed game server to get command execution directly on the server. A hacker would have to break into the root account first.  
  
Firewall blocks everything but SSH from hosts on local network and Minecraft.  
SSH restrictions  
  
For one thing, these protections don’t interfere with the game itself. If there’s an exploit in the game then there’s a risk your machine can still be compromised. For in game protections it’s smart to enable the whitelist and manually add the usernames of your friends to the list. That way random people won’t be able to access your server and grief you (destroy any builds)!  
  
In game as someone with the op role or from the server console, use the whitelist command to build a list of allowed users and enable it.

```
<pre class="wp-block-preformatted">/whitelist add <username>
/whitelist list
/whitelist on
```

### Is it enough?

Honestly, there’s a few additional things that could improve a server that I have not tried yet. Such as making a service for the game instead of relying on a script. There’s probably ways to make SSH more secure as well requiring 2FA through Duo. One of my first blog posts was on [enabling 2FA for SSH](https://www.hackerunder.dev/duo-securitys-2fa/). I’ll have to revisit that topic and show how it can work for this Minecraft server in a future post.

Minecraft runs on the default port unless you customize the server.properties file. Any port scanner will show that you’re running with port 25565 and a quick google will reveal that it’s Minecraft. There’s open source scanners like <https://github.com/Dinnerbone/mcstatus> that anyone can use to get information about your game. Like the banner, server status, latency ping, and number of players online.