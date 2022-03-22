---
author: Greenjam94
categories:
- Uncategorized
date: "2015-10-21T08:17:27Z"
guid: https://hackerunder.dev/?p=137
id: 137
tags:

title: Spartan Hackers Website
url: /spartan-hackers-website/
xyz_smap:
- "1"
---

Hello again, I am going to share my love hate relationship with my latest web design project: [spartanhackers.com](http://spartanhackers.com) Some background information, Spartan Hackers is a group at Michigan State University that holds weekly events to introduce students to various technical skills that they can use at hackathons like [Spartahack](http://spartahack.com).

## The Beginning

Spartan Hackers started up just last year, and only had a few members to run everything. The president at the time wrote a nice website for the club that was using only static content and the [grayscale bootstrap theme](http://startbootstrap.com/template-overviews/grayscale/). This worked great for last year, but the club is growing exponentially, I guess everyone wants to be a hacker. I’m the current webmaster for Spartan Hackers and it’s my job to maintain the website. However with my years of experience I thought I could spice it up and get the site working a lot better.

## The Redesign

The first major change I added was a quick new page to let people sign in to events. If you’re an old member, just type in your e-mail. Otherwise you have to complete a form with your name and major. This page has helped us a lot with tracking attendance and seeing how many people come back to events versus how many people are new. I really enjoy the concept of this modification… but in process of creating this page. I realized how sloppy I can be when I’m writing code at two or three in the morning. It’s a combination of Javascript, PHP, and MySQL with a lack of functions, classes, and clear structure (in my opinion). So before I made any more changes I started looking for a better alternative that would make the code more readable because I won’t be the web master forever, and I don’t want the next person to scrap my website design.

## Spartan Hackers get some help from Node

So in order to get what I want, I asked for some help from some front end developers where I work. They suggested I use [Node.js](https://nodejs.org/en/) because I needed something lightweight, easy to use, and I wanted to clean up the URLs by using [CRUD ](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)and [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) methods as well. The idea in my mind and the wireframes I showed to the rest of the eboard looks pretty sweet. We can track members, events, hackathons, all in the database. We can hook up APIs to social media and connect everything to the website instead of going to each account and posting about events. I have this great idea in my head for a grandiose website, but I am missing something… maybe time, or maybe the experience with Node. But right now development is at a halt.

## Challenges &amp; Future Goals

I’ve run into a couple of challenges. I based the website off of a guided tutorial I did at work to learn Node.js and that means that certain files were already created for me like server.js and folders were already created to manage the file structure. However, I’m at the point in my conversion where I am not sure what to do next. I want to continue building the site and making it functional, but this is a road block I haven’t been able to get past. Any help would be appreciated, you can find it on my Github at [this repo](https://github.com/Greenjam94/spartanhackers.com).

The future of this website is unknown. The e-board transitions in December which means I only have two months (on top of college and two part time jobs) to get past that road block and finish development. I really want to crank this out and show everyone what I have planned, so that I can prove my website development skills to my classmates. In retrospect, switching to a new framework probably wasn’t my best idea. But the temptation of how awesome this website could be made me do it. I’ll keep you posted on whether or not I finish this project by December.