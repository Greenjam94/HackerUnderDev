---
author: Greenjam94
categories:
- Uncategorized
date: "2015-12-04T12:01:36Z"
guid: http://blog.greenjam94.me/?p=229
id: 229
tags:
- blog.greenjam94.me import
title: Python Anywhere
url: /python-anywhere/
xyz_smap:
- "1"
---

I wanted to do a quick write up of the last project I did for class, it was a scoreboard app written in Python and used Flask. For extra credit we could host it on Python Anywhere. The app also uses SQLlite for the database. All of the development work was done using PyCharm.

One of my Media and Information classes has a lot of programming based projects. We used two main different languages and IDEs to get them done, the final for the class and the early projects was done using C# and [Unity](http://unity3d.com/get-unity/download), while the end of the class utilized Python and [PyCharm](https://www.jetbrains.com/pycharm/) from JetBrains.

## Development

All of the backend of the project was written in a single python file, it had database queries for getting, updating, and deleting scores. There are different sort options for the score board, each field is sortable. The project was pretty easy as far as homework goes, the instructor gave us a list of things to do and had a guided example. The only work I had to do by myself was some extra credit improvements. I added a image for the delete button and made a custom CSS file to work with Flask and make the front end look better.![PyCharm Screenshot](http://blog.greenjam94.me/wp-content/uploads/2015/12/Screen-Shot-2015-12-22-at-12.29.10-PM-300x188.png)

## Debugging

PyCharm makes debugging easy, everything can be run on your localhost, which means you don’t need to update a server each time you need to test code. By specifying the port number, you can run the website on your own computer without hassle.

## Hosting

Hosting on Python Anywhere was really easy. I created an account and verified it through email. Then I uploaded my files from PyCharm and ran the database init commands through the sites python terminal. After that all I had to do was go to [greenjam94.pythonanywhere.com](http://greenjam94.pythonanywhere.com/) and you could see the scoreboard.

<figure aria-describedby="caption-attachment-231" class="wp-caption aligncenter" id="attachment_231" style="width: 268px">![Python Anywhere Screen Shot](http://blog.greenjam94.me/wp-content/uploads/2015/12/Screen-Shot-2015-12-22-at-12.30.18-PM-268x300.png)<figcaption class="wp-caption-text" id="caption-attachment-231">Screen shot of the web app.</figcaption></figure>

Feel free to play around with the board, all I ask is that you don’t delete all the records.