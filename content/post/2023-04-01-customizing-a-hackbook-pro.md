---
author: Greenjam94
categories:
- Tools
tags:
- macOS
- kali
- VMware
- Wazuh
ct_period_last_updated:
- default
date: "2023-04-01T00:00:00Z"
title: Customizing a hackbook pro 
url: /customizing-hackbook-pro
featured_image: '/images/hackbook-pro/kali-2023_background.png'
---

To develop in cybersecurity, it is vastly important to keep a growth mindset. Always be learning. In order to allow that, it helps to have the right tools available. One important tool is a dedicated computer for hacking activities such as participating in training labs, testing tools, or learning new methodologies. I reset a Macbook Pro, keeping the intending macOS and installed VMware fusion for running virtual machines (VM) including Kali linux. Since the purpose of building this Macbook is to run VMs for hacking, I am calling this my Hackbook! Phillip Wylie documented his mac setup while [creating a pentesting rig](https://phillip-wylie.medium.com/getting-started-with-pentesting-rigs-28a512f50f35), this is a very similar approach to what I took.

# Why macOS and VMware?

There are too many options these days when creating a hacking environment. Cloud based virtual machines, installing linux directly onto old computers, building small form factor machines as [red team implants](https://github.com/sean-t-smith/T-Mobster_Implant) are all viable with pros and cons of their own. I chose to keep macOS because of my familiarity with how it works. Mac's preferences, power usage, and terminal make it ideal for my needs. I also enjoy how screenshots work in macOS compared to windows. Having screenshots automatically save to a folder on the desktop makes it easy for organizing evidence.

To host the various virtual machines I installed VMware Fusion. A software hypervisor for macOS that allows me to create specific machines without needing separate devices. VMware has worked well for me in the past. The networking options are easily updated and creating shared folders between the host and VMs are simple to set up. One major benefit to running virtual machines is the ability to take snapshots of machines, allowing one machine to be rolled back if required. 

# Customizing a Kali VM

Kali Linux just turned [10 years old](https://www.kali.org/blog/10-years/)! Along with some new developments such as Kali Purple, the team published a new release [2023.1](https://www.kali.org/blog/kali-linux-2023-1-release/). They prepackage it into images specifically for VMware or virtualbox. After downloading and installing the image, I got to setting up the machine to my liking. First task was resetting the password, these images have a default user and password of kali. Running the command `passwd` allows me to overwrite that. As a touch of personalization, I updated the hostname to match my theme of using pokemon names. I set the mac's name to Dragonite and the Kali VM to Dratini. Kali has a few [new background images](https://www.kali.org/blog/kali-linux-2023-1-release/#all-new-wallpapers) for use, so I switched to the TJnull inspired one.

In the VMware settings, I updated the machine to use more available memory and created the shared folder in case any files has to be exported from the VM. I also created a snapshot to use as a safe rollback point after getting everything as I liked it.

![kali hostname and mounted folder](/images/hackbook-pro/kali_hostname_and_mount.png)

In my last blog post, I checked out [Wazuh](/wazuh-open-source-security-platform) an open source security platform for monitoring computers. I added an agent to the kali VM to compare how VMs report vs dedicated machines. Even with a fresh installation, there were a few default configurations identified as issues by the agent, for example allowing insecure properties of SSH. After updating the sshd_config, I was able to force the Wazuh agent to scan the VM again by restarting the service using the command: `sudo systemctl restart wazuh-agent`.

![wazuh agent connected](/images/hackbook-pro/wazuh-agent_connected.png)

## Hacker education

The main use for the Kali VM will be to access different training platforms such as OffSec's Proving Ground or Try Hack Me. To ensure I can access both, I logged into each platform and downloaded a openvpn configuration file for each.

OffSec mainly provides courses, but fewer know that there is also independent labs available. Proving Grounds do not provide educational content directly, but high quality machines to target and attack. Targets are created for you in a private network, other users on the platform are not using the same machines. There is a free tier called "play", where machines can be spun up for 3 hours each day. Otherwise the paid tier "practice" is available with a monthly or yearly subscription.

![offsec vpn success](/images/hackbook-pro/kali-offsecvpn.png)

Try Hack Me is another platform for learning cyber security. Try hack me also has vpn access like Proving Grounds, but their content is organized into rooms and learning paths. Try Hack Me offers content beyond penetration testing. Some rooms are built around devops and blue team as well.

![tryhackme vpn success](/images/hackbook-pro/kali-tryhackmevpn.png)

# Bare metal vs Virtual Machine

Most of this article has been focused on setting up a Kali virtual machine. Why not install Kali linux directly onto a physical device? There are many applications becoming stable on linux distributions, but usually most products are better supported on Windows or macOS. Having virtual machines allow me to build, test, reset, and destroy instances as required. With this in mind, there's a few applications I wanted to have persistent and not reliant on temporary machines.

On macOS, I wanted to have Discord so I can easily keep in touch with community while working on things. I can use the host for long term storage, especially things like notes or drafted blog posts. Speaking of notes, I installed Obsidian to track playbooks and write ups as I explored. The obsidian vaults are backed up to google drive so I can access notes from various machines.

Earlier in this post, I explained how I customized a Kali VM. Kali tools, platform access, and agent testing is my intended use for that machine. Each VM can be for unique purposes as needed in a sandbox environment.

# Conclusion

My hackbook pro is a mac0s system with VMware fusion supporting virtual machines. Keeping macOS on the physical device allows me to use a wider range of applications outside of virtual machines. Discord, Obsidian, macOS terminal and mac's screenshot functionality is a sample of those uses. Through VMware, I created a Kali virtual machine. On Kali I customized it to my liking and installed a Wazuh agent to monitor it. Offsec and Try Hack Me VPN files were downloaded and tested for future hacking fun.