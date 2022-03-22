---
author: Greenjam94
categories:
- Uncategorized
ct_period_last_updated:
- default
date: "2019-03-16T23:37:10Z"
guid: https://www.hackerunder.dev/?p=759
id: 759
image: /wp-content/uploads/2019/03/computer-hand-keyboard.jpg
tags:
- password
- WordPress
title: Replacing a forgotten WordPress password
url: /replacing-a-forgotten-wordpress-password/
---

What is the best part of creating a new blog? You create everything, move content, and then get back to the daily grind. Come back to write the next post and, wait, what did I set as the WordPress password? Looks like we’re going to have to overwrite the hash in the database.

<div class="wp-block-image"><figure class="aligncenter">![WordpressLoginError](https://www.hackerunder.dev/wp-content/uploads/2019/03/WordpressLoginError.png)<figcaption>WordPress Login error</figcaption></figure></div>While I didn’t have access to the administration pages of WordPress. I did use an SSH key when setting up the server. A file that makes it so I do not need a password to get access to the web server and database. Ideally, it would be as simple as logging into the database, finding the users table, and updating a value (the password hash) to the new password I wanted to use (Spoiler: it is). Below is how the hash we are replacing looks when inspecting the wp\_users table.

```
<pre class="wp-block-code">```
mysql> SELECT ID, user_login, user_pass FROM wp_users;
+----+------------+------------------------------------+
| ID | user_login | user_pass                          |
+----+------------+------------------------------------+
|  1 | admin      | $P$BThiRip7s2lXh/PBVW7yFnKbQWvDtc0 |
+----+------------+------------------------------------+
```
```

Here’s the problem though, we need to know how WordPress hash passwords in version 5.1.1 so the website can correctly use it while logging in. WordPress used to use MD5 hashes in previous versions. However looking at the value of user\_pass above, there are a few characteristics that should make us question if this is a MD5 hash or something else. An MD5 is typically represented as a sequence of 32 [hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal) digits, which means the only characters should be 0-9 and a-f. In the value above, we see special characters and other letters. So what does WordPress use if not MD5 hashes?

## Default hashing functions in WordPress

WordPress now uses [Portable PHP password hashing framework](http://www.openwall.com/phpass/) to make more secure passwords. Based on multi-level MD5 hashing and salt values, passwords are more securely stored than a standard MD5 hash. The problem with just hashing a value, is every time you hash a value, the output is the same thing. For example, if an admin has a password **spring2019** and a user creates an account using **spring2019** as well, then user\_pass in the database would have **17d74cf4308267eb9e56edfbd36b8c86** for both users. If the database with MD5 values was ever access by a hacker, it would be easy to see multiple users that used same password. That’s where salting comes into play. “Salting a hash” adds a unique value to the password before the cryptographic algorithm is applied. This ensures no two database values are the same.

So for those of us who can access the database to reset our forgotten passwords. What is possible? Somehow we have to figure out a valid salt, then learn how to use Portable PGP hashing to match the format that is already in the database? Not at all! Thanks to the developers at WordPress, there is backwards compatibility that supports unsalted MD5 hashes.

> When a user logs in with such a password \[where an unsalted MD5 hash is stored in the database\], WordPress detects MD5 was used, rehashes the password using the more secure method, and stores the *new* hash in the database.
> 
> <cite>https://ttmm.io/tech/wordpress-password-hashing/</cite>

## The solution to a lost password

In order to regain access to the administration pages, there’s only one SQL query required. New versions of MySQL or Maria databases support MD5 hashing, so we can hash a password as we store it in the database. The command is shown below. In order to get the ID of the hash you’re replacing use the SELECT query from earlier in this post.

```
<pre class="wp-block-code">```
mysql> UPDATE wp_users SET user_pass = MD5('spring2019') WHERE ID = 1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```
```

While logging in, the MD5 hash in the database is recreated using WordPress’ salted hash functions. Now we have access and can continue to write blog posts! However, MD5 hashes whether salted or not, are not recommended as secure hashes anymore. Is there a way to use a stronger hashing function with our WordPress user accounts?

## Further security

MD5 hashing should be replaced by newer algorithms. [OWASP provides some great information](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Password_Storage_Cheat_Sheet.md) on what should be used for password storage in modern web applications. WordPress has also designed the hashing functions to be “pluggable”. This means that plugins can overwrite what the function does. So plugin developers can provide better solutions like [wp-argon2-password-hasing](https://wordpress.org/plugins/wp-argon2-password-hashing/) or [wp-password-bcrypt](https://github.com/roots/wp-password-bcrypt). The trade off is that such plugins are maintained by the community and have less support. For example, the Argon2 plugin has fewer than 10 installations and the bcrypt plugin hasn’t been tested against the last 3 WordPress updates. As a website owner, you need to balance the risk of using an outdated hashing algorithm or a poorly supported plugin.

[Wordfence](https://www.wordfence.com/) is another great WordPress plugin that can aid security. When I first reset my password, I purposefully used a weak password knowing I would change it again using the administration pages. I got an alert across the top of the page saying my hash was insecure, this is a great way to advise users when they have created a poor password. Wordfence also has additional rules that can be setup to help secure your website. Failed login attempts are counted and certain usernames can be set to instantly ban the attacker’s IP.

<figure class="wp-block-image">![Insecure password alert](https://www.hackerunder.dev/wp-content/uploads/2019/03/WordFenceAlert.png)<figcaption>The alert message from Wordfence when using an insecure password</figcaption></figure>## Disclaimer

The database output was modified, and the examples above are insecure. No one should be using passwords like **spring2019**. When creating a password, a longer “passphrase” not based on a single dictionary word is preferred. [NIST](https://pages.nist.gov/800-63-3/sp800-63b.html) now recommends using a single \*secure\* passphrase instead of rotating passwords all the time.

## Conclusion

WordPress passwords can be overwritten in the database with an unsalted MD5 hash. This allows an admin to regain access to their website. WordPress now salts passwords; using an additional value to make hashes unique and the database slightly more secure. There are plugins available to use a stronger hashing algorithm, however they are not widely used. The Wordfence plugin allows for more security controls like alerting to weak passwords and locking out a user after 3 failed login attempts. Finally, use a secure passphrase; the longer the better and do not worry about changing it often. Just try to make something you will not forget!