---
author: Greenjam94
categories:
- Community
date: "2016-12-01T23:22:44Z"
guid: https://hackerunder.dev/?p=435
id: 435
tags:
- slack
- IRC

title: Setting up Slack for MiSec
url: /setting-up-slack/
---

Some time last year, I wrote a post about setting up an IRC client on my VM. The idea was that since it’s always online, I’d always have the chat history for the #misec IRC channel. That way I’d never miss a mention or interesting conversation.

Since then, a lot has changed and I don’t connect to that machine as much as I used to. I had to restart it a few times so the “always online” theory quickly fizzled out as well. I found that a majority of my MiSec conversations were on twitter or in person.

## Why Slack?

At the RuCTF, we used misecredteam.slack.com to transfer notes and share files. For those that don’t know about Slack, it’s a modern chat client. While it may be just another messaging app to some people. I’ve used it through college, at work, and for groups like [MiSec](http://michsec.org/) and [lansing.codes](https://www.lansing.codes/). There’s been talk about trying to get an official MiSec slack channel.

During the November Lansing social, we did just that and misecgroup.slack.com was created. Later that night I found a [project](https://github.com/outsideris/slack-invite-automation) on Github that had a “push button” solution for creating a auto-invite application on heroku.com. Shortly after setting that up, I was able to tweet out the URL and people starting joining the new channel. If you’d like to set up a similar invitation application, then read the Github description and press either the Heroku or Azure deploy buttons based on what service you want to use to deploy the application.

## How it works

The app works great. Heroku even took care of a lot of the hosting details, like handling TLS. Within a day, the channel had 30 members and I didn’t have to manually invite anyone. The only change I made to the app was cosmetic. I didn’t like the gradient background so I replaced it with a more “cyber” background. In order to change the application, I had to fork the github repository and connect it to my Heroku app. I used git and the Heroku CLI to do the heavy lifting. To change the background I simply replaced the bg.jpg in the images directory and redeployed the app.

## IRC or death

A lot of MiSec members prefer to stay on IRC. In an attempt to accommodate their preferences, I opened an IRC gateway to connect to the channel from their favorite IRC client. However that still requires to be on the #misec IRC channel and the irc channel for MiSec slack… The only thing more annoying than having to be in multiple chats is being in multiple chats for the same reason.

So I found an alternative with the help of some MiSec friends. Another Github project called [slack-irc](https://github.com/ekmartin/slack-irc). The bot uses nodeJS to run, so hopefully anyone attempting this themselves have some experience with npm. Slack-irc made it possible to set up a slack bot that integrates with another IRC channel. So now #misec is in misec.slack.com’s #general channel and vice versa.

<figure aria-describedby="caption-attachment-440" class="wp-caption aligncenter" id="attachment_440" style="width: 944px">[![Demo from GitHub, show's how it looks for each client.](https://hackerunder.dev/wp-content/uploads/2016/12/slack-ircDemo.gif)](https://hackerunder.dev/wp-content/uploads/2016/12/slack-ircDemo.gif)<figcaption class="wp-caption-text" id="caption-attachment-440">Demo from GitHub, show’s how it looks for each client.</figcaption></figure>

## Becoming a Slacker

If you’re interested in joining the MiSec slack channel, follow the steps below:

1. Get an invite by going to [misec.herokuapp.com](http://misec.herokuapp.com) and entering your email address you’d like to use for the account
2. Finish creating an account for the channel  
    (Please note the team URL is misec.slack.com)
3. Sign in from a Slack application on whatever device you prefer if you don’t want to use the web client.
4. Optional: Go to [https://misec.slack.com/account/gateways](https://misecgroup.slack.com/account/gateways) for instructions on connecting over IRC