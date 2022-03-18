---
author: Greenjam94
categories:
- Uncategorized
ct_period_last_updated:
- default
date: "2022-03-17T10:08:27Z"
id: 952
title: Hello Hugo, Goodbye WordPress
url: /to-hugo-from-wordpress/
---

One of my goals for 2022 is to blog more frequently. I used to try and get a post out weekly when this blog was originally a college project. Weekly turned into monthly posts as my content got more technical and my career took off. Last year I posted once. Now that ends! I'm kicking off this revival with a site refresh. HackerUnderDev is moving away from a WordPress site running on a DigitalOcean droplet for a Hugo site running on Netlify.

## Goodbye WordPress

There were a few issues with how I previously ran my blog.

1. HTTPS certificates had to be manually renewed
2. WordPress requires occasional updates and maintenance
3. Comments were allowed, but mostly recieved from bots
4. DigitalOcean costs money to maintain

My main reason for switching is to learn something new and revitalize my interest in blogging. However Security, Cost, and Performance are all great reasons to switch too. WordPress is a Content Management System (CMS) built in PHP. It's a powerful framework with a large community, mulitple plugins, and many sites use it. However there's plenty of examples of WordPress being a common target for hackers because it's so commonly used. Being a full CMS also means it requires a database for storing content, user activity, and more. There's a largr attack surface on a website with so much functionality and my site wasn't making use of all those features.

## Hello Hugo

Running Hugo on Netlify offers solutions to each of these. It's a minimal but functional blogging alternative. Hugo is designed to provide static web content, text, images, and external links. That's all I need in a blog.

1. Netlify builds and deploys with HTTPS automatically
2. The site is static and once built requires minimal care
3. Static sites also don't allow comments so there is less to moderate
4. Netlify has a free tier for hosting

## Hugo's not perfect 

As I test things out I am finding that Hugo is not the easiest WordPress replacement. Some of the issues are painful, but as I learn the process and how to use Hugo I expect it to get easier.

WordPress had a built in editor but Hugo converts files into web content. So in order to create a new post I need to add a new file to the code repository and rebuild the site. Hopefully by using Github's online code editor this will still allow for easy posting. 

Plugins existed in WordPress that do not for Hugo. For example YoastSEO to track word count, grammar, page layout and other requirements for strong Search Engine Optimization.

Exporting 6 years of WordPress content is not a smooth transition. I've attempted this once before with Jekyll and had a similar experience. While the blog posts usually get transferred completely, the formatting is corrupted and images are lost. This requires a manual review of all the previous posts to correct any mistakes.

All in all, I'll take some extra edits and a learning curve if it means a cheaper, more secure, and faster loading blog. I'm hoping that you reading this can bear with me as I transition. Please reach out on Twitter to @hackerunderdev or post an issue on Github with any comments or concerns!