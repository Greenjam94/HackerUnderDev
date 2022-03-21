---
author: Greenjam94
categories:
- Uncategorized
date: "2016-03-07T15:45:22Z"
guid: https://hackerunder.dev/?p=277
id: 277
tags:
- blog.greenjam94.me import
title: Social engineering a hackathon
url: /social-engineering-at-a-hackathon/
---

When a lot of people hear about hacking, they imagine a guy in a hoodie at a computer late at night. That’s not always the case, social engineering is a big part of the picture. This last weekend I went to [Bsides Indy](https://www.bsidesindy.com/), and the keynote was about communicating with management about security. He gave a few examples about breaking into some of the most secure places… because of human error.

This got me thinking, is there anything I’ve done that is close to that, and a good example is my time volunteering at a hackathon a few weeks ago. Last year was the first SpartaHack, and I volunteered there as well, I spent the whole weekend carrying boxes, helping hackers, and doing anything asked of me by the organizers. Originally I was going to help organize the web services this year, but plans changed for certain reasons. I ended up offering to volunteer again last minute before the event started.

Since it was last minute, I jumped into helping where I could and I still had access from the early planning days of the event. So I caught up on all the information about volunteering and helped out. When I first arrived at the event, I was told to go grab a t-shirt, so I went to the volunteer HQ and grabbed the first one I saw. Apparently I got an organizer shirt, so I was well on my way into social engineering territory, even if it was just a quick snatch and grab. If I wanted, I could’ve grabbed a radio too and started talking to hackers and just making things up trying to get to whatever I wanted.

While I was volunteering, I was “general help” or “help desk” most of the time. The first job meant that I walked around and helped wherever I could, the second job was sitting behind a desk and helping hackers by answering questions or returning hardware that was lent out.

Near the end of the event, I was picked to help with judging and was giving admin access to the submission portal of everyone’s projects so I could approve them. So just because I was sitting in the right place, and raised my hand to try and help, I was given admin access to the event just to approve projects.

Now since this was something I was doing at school, I didn’t really try to break anything, I didn’t want things to go bad or anything like that. But for example, what if I put on my “hacker’s hoodie” and wanted to play around? There was a lot I could have done. For example, I was helping with lending out hardware like Raspberry pi’s and Arduinos… what would happen if a one went missing? Who’s held responsible? I was approving projects, what if instead I started deleting them, so 1/3 of the projects didn’t get included for judging. I realized I didn’t have access to the production databases, but the dev one. I could still have used that access to test SQL injection against the dev environment and then use these exploits in on the real site. Even more simply, I could have used one of the organizers laptops in the HQ to get access, laptops were left out and unlocked. I’m sure it wouldn’t have been hard to drop a couple tables. Again, I didn’t do anything of this sort at the hackathon, I’m just saying that it would have been possible.

So how important is social engineering? Very. Humans are the easiest part of a system to compromise. Even in the most secure environments, if someone forgets the rules or makes an exception, something can go awry. Social engineering can be as simple as wearing the wrong t-shirt, and complex enough to imposter an auditor at a major company. If you want to get better at social engineering, practice telling stories, the more convincing and believable the better. For more on social engineering, [look here](https://www.owasp.org/index.php/Social_Engineering).