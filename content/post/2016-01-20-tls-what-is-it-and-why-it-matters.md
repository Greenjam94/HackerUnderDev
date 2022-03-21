---
author: Greenjam94
categories:
- Uncategorized
date: "2016-01-20T15:21:16Z"
guid: https://hackerunder.dev/?p=238
id: 238
tags:
- blog.greenjam94.me import
title: 'TLS: What is it and why it matters'
url: /tls-what-is-it-and-why-it-matters/
xyz_smap:
- "1"
---

In my normal fashion, I’m going to start this blog post with a little intro to cover my butt. Recently at work, I’ve been tasked with learning about Transport Layer Security or TLS. This blog post is my own thoughts and is not 100% accurate, but I hope you get the idea as well as I do.

## What is TLS?

Well, as I said above, TLS is Transport Layer Security. It’s the encryption used by clients and servers to encrypt messages sent between the two. Some of you may remember SSL, or Secure Socket Layer. That was the predecessor of TLS. Since then, SSL has been proven to be insecure; don’t ask me how, but I do know that one example of abusing SSL is the POODLE attack. [Wikipedia](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0) says “all block ciphers in SSL; and RC4, the only non-block cipher supported by SSL 3.0, is also feasibly broken”. That’s about as far back as I went with the history of TLS. All I took away from SSL is that it’s deprecated and no one should use it.

TLS encryption is a complex combination of keys, certificates, and ciphers… I’ll try to explain this but don’t punch your screen or write me an angry email if I’m wrong. Before any website traffic is sent between a client and a server, they must agree on a key and a cipher. The cipher encrypts the messages, the key decrypts them. The client and server exchange keys using a fancy algorithm and decide on a cipher that both know how to use. Certificates are like ID’s that prove a TLS connection is valid.

## Why does TLS matter?

Have you ever looked at a packet as it goes across your network? If not, I suggest looking at [mitmproxy](https://mitmproxy.org/). A quick rundown for the non-network people. A request over regular unencrypted HTTP traffic is visible to everyone on the network, a hacker can grab all of the information in your request, like your password, user tokens, credit card, email address, etc. An example of a HTTP request is below from [shakedos](http://www.shakedos.com/2013/Nov/23/tinder-privacy-issues.html).

<figure aria-describedby="caption-attachment-239" class="wp-caption alignright" id="attachment_239" style="width: 300px">![Lack of TLS reveals auth token](https://hackerunder.dev/wp-content/uploads/2016/01/mitmproxy-tinder-token-9-300x134.png)<figcaption class="wp-caption-text" id="caption-attachment-239">http://www.shakedos.com/2013/Nov/23/tinder-privacy-issues.html</figcaption></figure>

This is mitmproxy catching a pair of authorization tokens for the Tinder “dating” app. Now that the hacker has your auth token, they can inject it into their own request and gain access to your account. This is one example why it’s dangerous to send sensitive or private information over encrypted channels. So to protect your user’s information, encrypt your traffic! If this was all sent over HTTPS using TLS, then the information would not be decipherable without the client’s key.

## Why everyone should use encryption

This isn’t exactly correct… but I agree with the principle. Somewhere, probably twitter, I read a couple articles linked from cryptographers about the point of early cryptography. Spies would email home over encrypted channels. While the message is unreadable, it is still sent out over the network. The only difference is instead of seeing “token: abc123” it’s “W$JT#N:SNV120934”. Network monitors that expect to see unencrypted requests can flag a request with unknown contents and trace the source. aka if our friends, the North Koreans caught an encrypted e-mail and trace it to a coffee shop, chances are they can find our spy. Did that make sense? This is the reason why everyone should encrypt things, so ALL the internet traffic is a jumbled encrypted mess, and that monitors can’t single out a specific source of an encrypted message.

## Things to shoot for

There’s a lot of configuration options for TLS, even the keys are complex. The main thing to remember when setting up TLS keys is that it’s important to have a minimum of 128 bits of security. Which means RSA keys with 2048 bits or ECC keys with 224-255 bits. You can also have larger keys, but going larger is pointless if you use Elliptic Curve ciphers (ECC). In order to save time, OpenSSL stores keys on the server from clients that have already shared keys with it. These keys are encrypted using AES128-CBC and that is only as strong as 128 bits of security. Because of this, it’s also good idea to restart your server nightly in order to reset the cache of stored values.

Certificates are also important, the signature should use SHA-256 or better. Use a name that fully represents the name of the domain. Don’t use “www” or “localhost”. Certificates signed with a certain encipherment (or signature) will use the same cipher to encrypt messages, so ECDSA certificates will use ECC ciphers. Server certificates should be X.509 version 3, older ones are deprecated. Use certificates issued by a CA, not self-signed certificates. They publish information that traces a line of authority and that can be used to authenticate the certificates. If your site uses personal identity information (PII), then it’s a good idea to have a EV certificate. It’s a special TLS certificate with your companies name on it to “increase user confidence in that the site is who it claims to be”.

When defining your cipher list, it’s a good idea to only use the latest recommendations from NIST. Thanks to [Mozilla](https://wiki.mozilla.org/Security/Server_Side_TLS), A good example of a modern cipher list is …

**ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!3DES:!MD5:!PSK**

… This cipher list gives priority to the first ones defined, so more secure cipher are used compared to weaker versions. The last line prevents broken or deprecated ciphers from being used. Again, SHA-256 or higher is recommended and SHA-1 shouldn’t be used. It’s important to note that AES-128 is different and I believe it’s ok to use ciphers with that in it’s name. Favor GCM ciphers over CBC ciphers because it is an authenticated encryption with associated data.

## Try it out!

You’re probably thinking “putting encryption on my server is hard, and if I’m going off your blog, I have no idea HOW to do it, just why I should”. However setting up TLS on your sever can be really easy thanks to a new software called [LetsEncrypt](https://letsencrypt.org/). Depending on your server, it’s either really easy or kinda easy to set up. I have a Ubuntu VM and all I had to do was update Python to 2.7.9, run apt-get-update, clone their [github repo](https://github.com/letsencrypt/letsencrypt), and finally run **./letsencrypt-auto certonly –apache -d blog.greenjam94.me**. The software runs through it’s own installation of dependencies, it’ll open a GUI where you submit a email address and choose if you want to use http and https or just https, I suggest just https and being more secure. After that it’s all set up for you. Lets Encrypt sets up all the configuration you need, even for applications like WordPress Blogs. However I did have to change some of my links on each webpage, in a rush I made my connections to some CDNs (hosted code that I borrow) as http and that gave the browser mixed messages.

The downsides to using LetsEncrypt is that it’s a free program that obfuscates your online intentions. However, that also means it’s a free tool for hackers to hide their treasure chests of evil goodies. Google has started to notice this and is flagging some of the LetsEncrypt certificates, so chrome might tell your website visitors that you use a suspicious TLS certificate. I haven’t seen proof of that, but infosec friends warn me of this. It’s also limited to 6 month certificates, they expired pretty fast. All you need to do in order to fix this is re-run the command from above, but it’s still maintenance that needs to be accounted for. I’m sure there’s one or more sysadmins out there that have written a bash script or cron job to do just that.

## Testing TLS

There are a few ways to test servers for TLS. You can use free labs provided online, like [Qualys SSL labs](https://www.ssllabs.com/ssltest/analyze.html?d=greenjam94.me&latest). Another option is to use tools like [OWASP’s O-Saft](https://www.owasp.org/index.php/O-Saft). If you are a sysadmin or just love to play in the console; [bughunter](https://twitter.com/x1622sec) recently blogged about a [bash script](http://blog.x1622.com/2016/01/openssl-bash-script-to-check-server_9.html) that uses OpenSSL to check websites for TLS. I’ve used all three and it really comes down to what you’re looking for. All can confirm if you have it installed correctly, Qualys will give you a report card. O-Saft will give you some detailed information. Bughunter’s Bash Script will give you a list of all the available ciphers on that server.

## What TLS doesn’t solve

TLS isn’t a cure for cancer, but it is important. While TLS provides connection encryption, it doesn’t equal trust, validation, or security. TLS makes it harder for hackers on the network to see what packets you’re sending, however it’s still possible for them to do a man in the middle (MITM) attack. There are additional ways to prevent that like disabling TLS renegotiation and enforcing [HSTS](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security). TLS isn’t a replacement for a good implementation of user authorization or input validation. For example, TLS can’t stop a user from using SQL injection to drop your database tables if they can bypass authentication and impersonate one of your users. Their injections will just be encrypted as it comes flying towards your database. Just because a site has TLS doesn’t mean it should also be automatically trusted. Don’t give a site your SSN or credit card just because you see a lock in the URL window.

## TLS Takeaways

Here’s a quick wrap up. SSL is old, don’t use it. TLS encrypts regular website traffic and makes it safer. Everyone should use encryption so the few that need it don’t get singled out. 128 bits of security is ideal. Installing TLS is easy with LetsEncrypt, it’s not a perfect fit and there are downsides but it’s a quick start. Test your TLS make sure it works. I hope you liked this blog post, I really enjoy this topic and will probably be doing a presentation on it at Misec in the coming months. I’ll be learning more and revising this as I do, please reach out to me if you have any advice.