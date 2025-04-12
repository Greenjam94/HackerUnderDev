---
author: Greenjam94
categories:
- Tools
tags:
- Ubuntu
- Docker
- Portainer
ct_period_last_updated:
- default
date: "2025-04-12T00:00:00Z" # Replace Date
title: Spring cleaning starts with servers
url: /spring-cleaning-servers
---

When you start seeing Spring Cleaning posts on socials, why not take a day to start "cleaning" with your devices at home? I took the day to review my home lab server and validate it could still run the projects I wanted it to. Below is all the steps I went through while knocking off the cobwebs.. the server has not been online since October of 2024.

# The problem

Outdated server, lab was running Ubuntu 20.04.05 as the operating system version. While I was able to still get into the website for portainer, I was greeted with more outdated messages. the portainer container was 1.4. Before starting some updates, I purged multiple unused containers that existed on the server.

# The clean up

Validate what containers to keep, portainer and freshRSS were the only projects I didn't fully remove. One personal project was a photo library for the family, I was able to download the photos and export them before wiping the container.

Updating the server and portainer was simple via `apt update && apt upgrade`, `do-release-upgrade`, and getting the latest public portainer container image.

# The new

It's not worth freeing up a lot of memory without also brainstorming a few new projects.
I have seen some posts online about project discovery's nuclei scan templates and how flexible some of the remote detections were. It's a new project I wanted to learn more about, so let's throw it on the home lab. I also uninstalled the Wazuh containers that I posted about previously in order to do a fresh install where the docker compose file was managed through portainer.

I also want to validate that both RunZero Community Edition and FreshRSS are running as expected.

## Nuclei

I looked up the project documentation and saw how to deploy the docker image.
In Portainer, I created a new stack with the docker-compose from the documentation provided.

```yaml
version: '3.8'
services:
  nuclei:
    image: projectdiscovery/nuclei:latest
    container_name: nuclei-scanner
    restart: unless-stopped
    volumes:
      - nuclei-config:/root/.config/nuclei
      - nuclei-templates:/app/nuclei-templates
      - nuclei-results:/app/results
    command: -version  # Temporary command to verify setup
    networks:
      - nuclei-network

volumes:
  nuclei-config:
  nuclei-templates:
  nuclei-results:

networks:
  nuclei-network:
    driver: bridge
```

After confirming the containers spun up as expected. I replaced the command and created a targets file to scan against.

```yaml
command: -list /app/results/targets.txt -o /app/results/scan-results.txt
```

After making the change to docker-compose in the web editor, redeploy to apply changes.
Read output via ssh on the lab machine, `cat /var/lib/docker/volumes/nuclei-results/_data/scan-results.txt`

Portainer containers restart automatically after closing, so I updated the restart policy to none. This requires me to manually start the nuclei scanner container to kick off new scans.

## Wazuh

I restarted the git deployment from scratch.

- `git clone` the public repo
- `docker-compose -f generate-indexer-certs.yml run --rm generator` creates a /config file with certs

Just like with nuclei, I created a portainer stack
Paste docker-compose from the single-node/ folder into portainer and manually update all relative paths to full paths
    ./config to /root/wazuh-docker/single-node/config
Deploy the stack in portainer afterwards

I can access Wazuh web interface at this point and some devices with agents are still able to connect to the new instance, cool.

update password from default, the UI needed: admin / SecretPassword
Follow [wazuh documentation](https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html#wazuh-indexer-users) exactly.
Only difference for portainer was when executing commands on the indexer container, I opened exec prompt through portainer and copied in the last few commands.

## RunZero

Not really a local service, RunZero hosts a community edition and I have agents installed on some local machines.
Confirmed both explorers are connecting as expected.

Found a previous task that checked for a remote IP which as no longer in use. Removed that task and related asset findings from RunZero.

# Conclusion

With Nuclei scanning and Wazuh agents reporting in, I have a large number of opportunities to explore some security related open source projects. On top of understanding these projects better, I also want to explore what kind of automation I can achieve with them in a home environment. It is a good practice to review servers and what they are used for. I was able to delete gigabytes of data for projects that were gathering dust and this gave me a chance to ensure everything I wanted to have running was still up to date and connecting properly.
