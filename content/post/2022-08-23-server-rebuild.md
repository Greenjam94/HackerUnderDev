---
author: Greenjam94
categories:
- Just for fun
tags:
- Ubuntu
- Samba
- Minecraft
ct_period_last_updated:
- default
date: "2022-08-23T00:00:00Z"
title: Rebuilding my home server
url: /rebuilding-my-home-server
---

A long time ago, I built a computer and have since sparsely used it as a home server.
The last time I reinstalled an operating system on it, I installed Ubuntu Desktop Eoan 
and never remembers to update the machine. This blog post will serve as a record of what
I did to setup the machine again after wiping it.

Services I installed included
- ufw
- ssh
- ftp
- Minecraft
- Samba fileshare

While attempting to install samba on the old system, apt update failed.
After some brief research, the answer seems to be to overwrite the old system and install a new operating system.

I copied the minecraft server data onto an external drive and prepared for the new operating system.

Install process

1. Go to https://ubuntu.com/download/desktop and download the latest LTS version

2. Flash the .iso disk image to a USB drive using https://www.balena.io/etcher/

3. Install Ubuntu following the prompts onto the server with a screen and keyboard attached

4. Ensure the computer is using a static IP on the home network

5. Setup SSH to allow remote connections from other computers

```
sudo apt update
sudo apt install openssh-server
sudo systemctl status ssh
sudo ufw allow proto tcp from 192.168.0.0/24 to any port 22
```

6. Harden

```
sudo apt install unattended-upgrades
sudo apt update && sudo apt upgrade

# https://linux-audit.com/audit-and-harden-your-ssh-configuration/
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo nano /etc/ssh/sshd_config # follow above link

sudo ufw enable
```


7. Mount the external hard drive and setup Samba

```
sudo fdisk -l #List disks
lsblk #shorter format, list disks alternative
cat /etc/passwd #to confirm uid and gid

sudo mkdir /media/folder
chown uid:gid /media/folder
sudo mount -o uid=1000,gid=1000 /dev/sdb1 /media/folder # Attach the external drive to Ubuntu filesystem

sudo apt install samba
samba --version
sudo nano /etc/samba/smb.conf
#[share-name]
#   comment = Named Samba Share
#   path = /media/folder
#   read only = no
#   browsable = yes

sudo service smbd restart

#Firewall access
sudo ufw allow proto udp from 192.168.1.0/24 to any port 137
sudo ufw allow proto udp from 192.168.1.0/24 to any port 138
sudo ufw allow proto tcp from 192.168.1.0/24 to any port 139
sudo ufw allow proto tcp from 192.168.1.0/24 to any port 445

sudo smbpasswd -a user # User has to be a linux username
```

\\IP\share-name
Enter samba creds

8. Minecraft

Add oracle repo
```
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt update
```

Get Java `sudo apt install openjdk-17-jre-headless`

install screen `sudo apt install screen`

Allow in ufw firewall

```
sudo ufw allow proto tcp from 192.168.0.1/24 to any port 25565
sudo ufw allow proto tcp from 192.168.0.1/24 to any port 25566
sudo ufw allow proto udp from 192.168.0.1/24 to any port 19132
```

Add user to organize minecraft specific operations with default settings sets up a /home/mc directory to host all

```
sudo adduser mc
```

Get the server jar latest link from minecraft
https://www.minecraft.net/en-us/download/server/

wget it to the server

Start screen with a name `screen -S minecraft`

run the jar `java -Xms4g -Xmx8g -jar minecraft_server.jar nogui`
Be aware of the avaialble RAM to use with this config, set a minimum and maxium that is within the system's specs

# Conclusion

I was able to rebuild my home server to run on the latest LTS version of Ubuntu. The server is hosting a Samba fileshare and minecraft servers to enjoy with family.

This server still has room to grow, a local webserver could be set up or additional services started as well.

## External resources (Click at your own risk)
- https://linux-audit.com/ubuntu-server-hardening-guide-quick-and-secure/
- https://www.osradar.com/hardening-ubuntu-security-ufw/
- https://gist.github.com/der-daniel/8fcd5a37f578c466e762c79b7b648c79
- https://www.digitalocean.com/community/tutorials/how-to-create-a-minecraft-server-on-ubuntu-22-04

