---
author: Greenjam94
categories:
- Hacker101
tags:
- ZeroTrust
ct_period_last_updated:
- default
date: "2023-02-19T00:00:00Z"
title: Into to Cloudflare Zero Trust
url: /Into to Cloudflare Zero Trust
---

Cloudflare offers some amazing, and free, products to secure personal use, self-hosted applications and devices. Last week, I set up a server at home running docker containers. By using Cloudflare I can securely make those docker containers internet accessible. Previously in order to do that, I'd have to open a port through my home router and accept any traffic from the internet on that port. This is visible and allows a home IP to appear in use to anyone who scans it.

Cloudflare Zero Trust replaces the need to expose your infrastructure directly by using their "global edge". By routing DNS traffic through CloudFlare's product, it allows for secure Authentication, improved logging or monitoring, automatic encryption, remote browsing and much more. For further details, check out the [overview](https://developers.cloudflare.com/cloudflare-one/) page of their documentation.

# Exposing a self-hosted application

This article will cover how I exposed my docker container to allow external access. In order to follow along, two prerequisites are needed. Domain ownership where custom nameservers can be set. I know both google domains and namecheap are good registrars to use. The second requirement is a docker container running a web application. I won't cover the setup of the docker container here, but you should be able to access it by visiting it on the local network.

## Add a website domain to cloudflare

Create a free account at cloudflare.com, this will require some personal information like an email address and payment method. After that go to [dash.cloudflare.com](dash.cloudflare.com) and click Websites on the top left menu, then click the "Add a site" button on the Home page.

It's then a three step process to complete the website registration

- Select the payment plan as "Free"
- Confirm your current DNS settings and hit continue
- Change your name server with your registrar

Changing the nameservers requires logging into your website's registrar, for example go to domains.google for Google domains and edit your domain's settings to use a custom nameserver. Cloudflare provides two servers that need to be used, Cloudflare follows a <firstname>.ns.cloudflare.com for their nameservers. Save your changes to the registrar and click "Done" on Cloudflare. The change can take up to 24 hours but I was emailed about the change in about an hour.

## Add Zero Trust tunnel

Go to [one.dash.cloudflare.com](one.dash.cloudflare.com) and click "Access" and "Tunnels" on the left menu. Click "Create a tunnel" on the Tunnels page to get started.

Follow the web UI to build the tunnel in 3 steps

- Name the tunnel
- Install the connector**
- Route tunnel, choose a subdomain or URI path to direct traffic to the tunnel

**For Docker, the connector install sets up a docker container called cloudflared from docker hub. The website command gives a basic `docker run` command with required parameters, but on top of that I suggest adding `-d` to run the container outside of the current terminal session and `--restart unless-stopped` to ensure the tunnel attempts to restart itself if there's an issue.

After that, you will be able to visit the subdomain and the traffic will route through cloudflare and to your docker image.

# Conclusion

Cloudflare provides a free means to gain remote access to your self-hosted applications without publicly exposing your network. Where previously I had set up reverse DNS with no-ip.com and portforwarding through my router, now I'm able to set up a container and tunnel all traffic through cloudflare. If any external party scanned my infrastructure, there would be no exposed ports or services for these docker applications. The domain, if scanned, would reveal that Cloudflare is providing the content.

If you are running applications and want to open it to the internet in a secure way, Cloudflare Zero Trust Tunnels is the way to do it.
