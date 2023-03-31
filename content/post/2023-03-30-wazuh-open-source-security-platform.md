---
author: Greenjam94
categories:
- Hacker101
tags:
- Open Source
- Wazuh
- XDR
- SIEM
ct_period_last_updated:
- default
date: "2023-03-30T00:00:00Z"
title: Wazuh - An open-source security platform
url: /wazuh-open-source-security-platform
featured_image: '/images/Wazuh/wazuh-feature.png'
---

A friend and mentor in the field introduced me to [Wazuh](https://wazuh.com/). An open source, free to use tool for security. He was looking at the tool to understand some of it's inner functionality for his own projects, but when I was reviewing it with him I was impressed by it's feature set.

First off, being open source means that Wazuh has source code published online that anyone can use, review, or extend. On the Wazuh website, it promotes open source for being Flexible, scalable, no vendor lock-in, and no license cost. Free community support and trusted by thousands of enterprise users including NASA. Secondly, there is a installation version built with Docker. In a recent post, I explained how I [built a docker server](/build-a-docker-server) to run Portainer and a FreshRSS container. Testing Wazuh would be an excellent use of this server as well.

# What is Wazuh?

Wazuh promotes itself as a all in one security solution. The [platform page](https://wazuh.com/platform/) of the site lists the following capabilities.

1. Security Analytics
2. Intrusion Detection
3. Log Data Analysis
4. File Integrity Monitoring
5. Vulnerability Detection
6. Configuration Assessment
7. Incident Response
8. Regulatory Compliance
9. Cloud Security
10. Container Security

I have experience with penetration testing, vulnerability management, and building web applications. In my current professional role, getting a better understanding of the above capabilities could provide me with a wider range of experience or allow me to approach issues from a new perspective.

## How Wazuh works

Wazuh has a few key components. Agent software is installed on devices that are being monitored, these devices cold be servers, laptops, virtual machines, or cloud instances. These agents then send logs to a server cluster. Each cluster contains nodes that help analyze agent information and forward alerts to an indexer. A graphic website displays information from both the cluster and index to keep users information of the security events being monitored.

# Installing via Docker

The Wazuh [documentation](https://documentation.wazuh.com/4.4/deployment-options/docker/index.html) on how to do an alternative install using docker was very thorough. By following along and copying the commands, I was able to install docker-compose, clone the Wazuh git repository, and spin up a single-node instance. There was also some server configuration that I had to set first.

The commands I needed to run on my linux machine were:

- sysctl -w vm.max_map_count=262144
- curl -L "https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
- chmod +x /usr/local/bin/docker-compose
- git clone https://github.com/wazuh/wazuh-docker.git -b v4.4.0
- docker-compose -f generate-indexer-certs.yml run --rm generator
- docker-compose up -d

![Portainer container list](/images/Wazuh/portainer-container-list.png)

By this point, I had configured my linux server, installed dependencies, created certificates for TLS and started the docker containers. This creates three containers: a manager, indexer, and dashboard. To view the stack, port forwarding is set up so that the linux server redirects HTTPS traffic on port 443 to the dashboard. The default credentials are user admin and password SecretPassword. I strongly recommend setting a better password after verifying everything is working at this point by following the [additional security steps](https://documentation.wazuh.com/4.4/deployment-options/docker/wazuh-container.html#change-pwd-existing-usr).

## Setting up agents

Now that Wazuh is running, the next step is to install agents on devices. The Wazuh dashboard does a great job walking you through a selection of options. After selecting the target operating system, confirming the Wazuh IP address, and defining a name for the device being monitored, the dashboard will provide a curl or powershell command required to download an agent from wazuh.com and provide steps then install and activate it.

![Wazuh deploy agent page](/images/Wazuh/create-agent.png)

After copying the commands, the agent will link to the manager container and become visible in the dashboard after one or two minutes. Over time, security events and more will populate in the dashboard. In my single-node test, I was able to set up two ubuntu servers and a windows laptop for monitoring.

# Next steps

Wazuh claims to be an all in one security platform. The 10 capabilities promised provide a wide range of security needs. Getting these needs met for a home lab without having to pay vendor licensing is a huge win, if it can deliver. Often times the problem with open source projects is a limited life span. Projects lose developers, funding, or support from users. I want to evaluate the promises Wazuh offers and compare it to other known solutions that I can run in a home lab environment.

There is also an API available to interact with the Wazuh stack. This allows for custom automation and interactions. For example, a discord bot could be created to allow for chatops, log monitoring, or larger visibility without exposing the Wazuh stack itself to the internet. I hope to be creating a series on different Wazuh features in the coming months, please reach out if this is something you are beginning to follow. Any specific questions or ideas would be great as I am just starting with this project.

Honestly, I have not spent much time with logs or on the defensive side. Getting these events and alerts in one place will be a great learning opportunity for me and I am excited to face these challenges ahead.

![Wazuh dashboard](/images/Wazuh/wazuh-dashboard.png)