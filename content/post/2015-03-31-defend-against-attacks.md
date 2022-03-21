---
author: Greenjam94
categories:
- Uncategorized
date: "2015-03-31T14:11:12Z"
guid: https://hackerunder.dev/?p=68
id: 68
tags:
- blog.greenjam94.me import
title: Defend your website against SQL injection and XXS
url: /defend-against-attacks/
---

Hey everyone, so at work we’ve had a couple vulnerabilities pop up so I was privileged with writing this up and I wanted to share it with you. I hope you find it interesting! Sorry it’s such a long read. There’s two parts, one for SQL injection and one for Cross Site Scripting.

**SQL Injection** [Check out SQL injection on OWASP](https://www.owasp.org/index.php/Reviewing_Code_for_SQL_Injection)

SQL injection is, simply put, a user adding additional requests to your database calls. For instance if you ask for a search term and compare that term to the database with something like:

SELECT title, author, text FROM books WHERE text LIKE ‘%$input%’

Developers can defend against SQL injection by validating all inputs. This means that we use a function that strips away characters that is required to run SQL from that input.

validateInput() would be a function we define that removes characters such as  
\*, `, or “. Something else to put in your validateInput() function would be PHP’s [MySQLi escape function](http://php.net/manual/en/mysqli.real-escape-string.php). It’s also wise to not allow Users to type in what tables they want to access in the database. Use separate queries instead of a dynamic one.

One of the simplest ways to test your inputs is to add ” or 1=1” to the end of the input. If we put ”’something%’ OR 1=1”’ into the example above we get:

SELECT title, author, text FROM books WHERE text LIKE ‘% ”’something%’ OR 1=1”’ ‘

**Cross Site Scripting (XSS )**  
[Check out XSS on OWASP](https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29)

XSS is similar to SQL in the sense that a user is trying to add their own code to our website. This time instead of affecting our database, it’s adding a javascript script (or a similar tag) to our pages code. Do you know what this is below?

%3C%2Fh2%3E%3Cscript%3Ealert%281%29%3B%3C%2Fscript%3E%3Ch2%3E

It looks like a lot of gibberish to me! While we don’t expect our normal users to type this into our log-in field, a hacker wouldn’t hesitate. While it looks like nothing, this is actually the HTML escape characters for &lt;script&gt;alert(1);&lt;/script&gt;. The user just added their own javascript to our website!

Just like the input we use for SQL queries we need to validate all inputs! I can’t say that enough. If the page is grabbing something, we need to pass it to a function that strips out anything illegal. In XSS’s case, we want to remove anything that is or could be a tag.

Our validation function needs to remove all tags, and the easiest way to do that is with PHP’s [strip\_tags function](https://php.net/strip_tags)!

Want to see if you can add your own javascript to the website? Use the alert(1) from above. It’s harmless and has a nice visual to show you  
that your site is indeed vulnerable to XSS.

**In Review**  
\* In general, assume that all users are vicious hackers trying to break your brand new website.  
\* Do NOT use user input for HTML comments, scripts, attributes, or CSS. ”’EVER”’  
\* Consider hiding SQL errors on live sites, Hackers use the information to understand the database layout.  
\* When taking input from a user, minimize their possible choices as much as possible.  
Example: Ages – Should only be positive whole numbers, no special characters.  
Example: Email – Should be a string @ string . string. [Now might be a good time to learn regex](http://regexone.com/cheatsheet)  
Example: Names – Should NOT have punctuation or special characters.