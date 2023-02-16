---
author: Greenjam94
categories:
- Hacker101
tags:
- HomeLab
- Docker
- Linux
ct_period_last_updated:
- default
date: "2023-02-15T00:00:00Z"
title: Building a docker server
url: /build-a-docker-server
---

This blog post is a record of what I did to spin up a home server that uses Docker for various side projects and fun. One of the pain points of having projects that involve computer applications, websites, or code is that it needs to be hosted somewhere to run. Our personal computers are not usually online for projects that could be running all the time. Creating physical servers or paying for cloud hosting can get expenive fast for passion projects or proof of concepts. Relying on one server for multiple containers allow for sharing resources and easily segregating projects. Using open source projects also become easy to set up as well.

# Set up a physical server

Luckily I have old computers laying around that can be repurposed into physical servers. Instead of buying a new computer or building one, I was able to use what I already had. The only thing to worry about was setting up the operating system on the machine. Just like the server I rebuilt in 2022, I wanted the operating system to be Ubuntu. It is free to use, well supported by the community, and is easy to get familiar with.

- Download ubuntu server
- Create bootable USB
- Boot from USB and complete local install
- Ensure ssh works
- Join to network and set static IP on router

At this point the physical server is setup. Ubuntu is installed with default configurations and SSH works for remotely connecting from other computers on the network. Here I removed the keyboard and screen used to install and relocated the server to it's home next to the other server.

## Install Docker

The main use of this server will be to host Docker. This allows software or even complete operating systems to be installed in a smaller form factor that is easy to manage and maintain. For this part, I followed the commands recommended by Digital Ocean at https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04.

```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
```

Once docker was installed, in order to test it, a simple application has to run. `sudo docker run hello-world` will output if everything worked as expected

## Install Portainer

Portainer is a web interface for remotely managing docker. Instead of relying on SSHing into the server and running CLI commands, Portainer allows for quick access and monitoring in the browser. Their documentation provides easy steps to set up the application: https://docs.portainer.io/start/install-ce/server/docker/linux.

```
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
docker ps
```

Visiting https://server-ip-address:9443 will show an account registration page if the installation succeeded. After creating an account continue portainer setup by following the prompts in the browser
https://docs.portainer.io/start/install-ce/server/setup. If you take too long to create credentials and opt out of submitting data, the docker container may need to be restarted from CLI as root `docker retart replace-with-container-id`

Now we have a ubuntu server running docker with a web management UI

One of the main reasons I wanted a Docker server was to host an RSS aggregator to collect information security news. In the browser I followed these steps:

- Search Docker hub for FreshRSS
- Portainer: Add a container
- Instance name, default configs, manually expose specific port
- Click deploy

## Setup FreshRSS

The FreshRSS team has made setting up an instance very easy. After creating the admin account and playing with some settings, these are the steps I followed to customize my application.

- Make the theme dark, else risk losing hacker status
- Add Kali Linux RSS feed to see how adding a feed works
- Import an export from https://freshrss.infosecali.com/
- Review broken feeds
- Organize feeds into different categories

# Set up Cloudflare tunnel

In order to share the RSS aggregator tool with other hackers, we need to expose the application to the internet. One way to do this would be to set up reverse DNS and port forwarding on the network's router and allow external traffic to reach the Docker server. However a better alternative is to use Cloudflare's free tier of DNS protections and zero trust tunnels to create an encrypted path from a domain to the docker server that is not visible if something scanned the network router for open ports.

First, a domain is required. Google domains or namecheap.com are good sources for cheap options. The domain has to use cloudflares specific nameservers for DNS. After the domain is protected, the tunnel can be created. After naming the tunnel, Cloudflare offers a few installation methods, one being to deploy a Docker container with a single command. The command needed some edits for my use, two options were addded `-d` to detach the process from the CLI session and `--restart unless-stopped` to allow automatic restarts unless it was specifically stopped. Finally a subdomain had to be defined for this specific application. For this use, https://intel.hackerunder.dev was selected.

# Conclusion

With some open source and free tools, I was able to turn a computer that was gathering dust into a server for Docker containers that had a web management tool and encrypted secure access from the internet to specific applications. Building this server allows me to collaborate with friends on threat intellegence collection and other hacker news sources. In the future, this server could also be used to host hacking challenges, virtual machines, and more as long as there was a docker image available for that use.

I hope you check out https://intel.hackerunder.dev for security news. Maybe this article will inspire you to build your own machine or to check out the many possibilities of Docker. Thanks for reading!
