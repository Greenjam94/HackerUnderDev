---
author: Greenjam94
categories:
- Tools
ct_period_last_updated:
- default
date: "2019-03-11T01:04:02Z"
guid: https://www.hackerunder.dev/?p=747
id: 747
tags:
- overthewire
image: /wp-content/uploads/2019/03/abstract-code.jpg
title: The power of scripting
url: /the-power-of-scripting/
---

On March 9th, I was a part of an awesome class hosted by [@Ashioni](https://twitter.com/Ashioni) that went over the Bandit challenges from [OverTheWire.org](http://overthewire.org). While I’ve attempted the Bandit challenges a [few years ago](https://www.hackerunder.dev/overthewire-bandit/), there are new additions and it’s always good to review how to answer these puzzles. There is never a single solution! In this article, I want to show a few ways we attempted the last challenge of the day. This will show how scripting can reduce the time required to complete a task.

The challenge was to figure out how to brute force a 4 digit pin. Providing the correct pin to a service gives us the flag. The service had an additional security control that seemed to make all connections drop off after 30 seconds.

## The manual approach

The first hurdle of automating a task involves figuring out what steps are required. Bandit is based around command line tools on linux, for example [nc](https://en.wikipedia.org/wiki/Netcat), [grep](https://en.wikipedia.org/wiki/Grep), and [ssh](https://en.wikipedia.org/wiki/Secure_Shell). The challenge introduction did not suggest any tools but we figured it would be based off of what we had learned so far. Since the focus so far has been on linux tools, I chose to create a [bash script](https://linuxconfig.org/bash-scripting-tutorial-for-beginners). However any scripting language could be used to accomplish similar tasks like these. Check out [go](https://golang.org/), [python](https://www.python.org/), or [ruby](https://www.ruby-lang.org/en/) and see how they compare.

This challenge involved creating a connection to a listening service, sending a message and comparing the response. The response output was either an error message or a success message.

Connecting to the service was easy enough using nc, passing the first pin (0000) was as well. I timed myself doing a few attempts (including the time to create the connection). I got an average time of 14 seconds per attempt. With 10,000 possible combinations, that would be a day and a half of manual data entry.

## Writing the script

The only thing the bash script did at first was create a list of inputs. The script used a for loop to create all 10,000 combinations and save them to a text file. Using a single command I was able to send that input list and save the service output to a new file. This is where I noticed the consistency of a 30 second timeout on the service. On average there was 7,000 successful attempts before the timeout happened.

The first iteration of the script really decreased the time required. Manually typing all the combinations would be impossible. Now I was able to get 7,000 attempts in under a minute. However the timeout provided a new challenge that had to be overcome. Another student, [Bryan](https://tibblesnbits.com/), got the flag by splitting the combinations into two files. I changed the script to also do this, each input file with 5,000 combinations. By running the connection command with these scripts I was able to go through all of the combinations in just over a minute!

## Making improvements

Cool! By scripting all possible combinations, that means somewhere in the output there is an answer. Using the skills from earlier challenges, I wrote a few more commands to hide the errors and extracted the flag I needed. At the start of this blog, I mentioned manually trying all combinations would take a day and a half of data entry. Using the script to get all the expected input cut my time down to just over a minute. Including the time to extract the flag from the output, it took about two minutes in total. Is it possible to get it even faster?

Using the bash script, we saved time to gather all the input. However, a lot of the time was spent writing commands to process the output. The connection and the output processing can be included in the script to further improve speed. Appending the commands to the end of the script, I removed all manual input required. Running this new script, I was able to get the flag in 34.7 seconds.

## Unexpected difficulties

We already saw how the connection closes and broke the first iteration of the script. This 30 second timeout would also have played a part if we were manually entering numbers. However, dealing with a timeout is not the biggest concern while writing the script. The developers of the service I am targeting might not have foreseen such automated usage. Worst case scenario, they only tested the service works. Two manual requests would show the correct and error responses are working as intended.

A major issue we need to consider is Denial of Service (DoS). Where sending so many requests that the server gets overwhelms and can’t respond correctly. This could mean the connection crashes or worse, we lock up the server’s resources and prevent it from functioning entirely. Another version of DoS that we need to be aware of is a [Fork Bomb](https://en.wikipedia.org/wiki/Fork_bomb). A Fork bomb is where a process replicates until it completely uses up all of the systems resources. I could easily see a revision of the script to do this. If one connection is fast, why not try 100 and do it even faster? Logically it might make sense. Two connections with 5,000 attempts took 34 seconds, so 100 connections with 100 attempts each would *have to be*faster! It’s important to remember networking constraints and system limitations while scripting out tasks.

This challenge is related to brute forcing pin numbers. In other hacker based scripts there are other concerns we might want to account for as well. For example, another service could be counting the number of failed attempts and killing the connection after three bad attempts. Similar to getting a user account locked out after failed password attempts. The server could also be monitored by a blue team or Security Operations Center (SOC). Processing so many attempts in such short amount of time would easily alert defenders to the script. Sometimes, speed isn’t the only goal. We could want to automate a process and remain undetected by security controls.

## Conclusion

So what does this mean to you? I didn’t give you the answer how to solve Bandit so what is the takeaway? Humans are slower than computers. In order to minimize the time required to do something it is important to know how to make a computer do the task for us. Using a computer and individual commands is just the first step, being able to script a program is the next step to really save some time. I challenge you to take a look at a reoccurring task you do at work. Is this something you could tell a computer to do more efficiently? Writing scripts will provide you with a productive edge in everything from Capture the flag events to tasks at work.

Remember that speed isn’t everything. Consider network restrictions, connection timeouts, and security controls while implementing your scripts as well! Sometimes the key to a script is not just speed but also stealth.