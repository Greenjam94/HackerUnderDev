---
author: Greenjam94
categories:
- Best Practices
date: "2016-04-15T08:00:25Z"
guid: https://hackerunder.dev/?p=299
id: 299
tags:
- tls
- hpkp

title: Adding a HPKP Header
url: /adding-hpkp-header/
---

Before we try to add a HPKP header, let’s review from last week. I made a post about what HTTP public key pinning is. It’s a fingerprint that browsers use to compare certificates can warn the user if the certificate is from a different source, even if it’s trusted or from the same server. If that doesn’t make sense, check out the link to the previous post.

## Public-Key-Pins

A Public-Key-Pins header looks like this:  
Public-Key-Pins: pin-256=”…”; pin-256=”…”; max-age=###; include subdomains; reporting-url=”…”;

The required parts are pin-sha256, and max-age. The pin is where you add the actual pin and the age is the number of seconds the the pin is kept on a browser. It’s good to note that the policy will not work with only one pin in the header, at least one backup pin is required. I’ll go over creating pins further below. For testing keep age short like 10 seconds (I did 500), otherwise standards recommends a two month period (5184000 seconds). Both include subdomains and the reporting url are optional. the include is just a boolean, I set it to true just to be safe. I don’t have reporting set up so I do not include that url in my header.

## Getting a pin

In order to get a pin, you first need a certificate. So make sure you have TLS enabled on your site. Basically, you need to make sure **https**://your-website works and you get the green lock. An easy way to get TLS set up is [Let’s Encrypt](https://letsencrypt.org/), I wrote about that in my[ TLS blog post](https://hackerunder.dev/tls-what-is-it-and-why-it-matters/) a few months ago, scroll to the Try it! section. Once you have a certificate, you have two choices. First, you can use a [nice tool](https://report-uri.io/home/pkp_hash) and grab the pin generated for you (don’t worry, you’ll be sending this hash to everyone anyways, it’s ok if someone else generates it for you… as long as it’s valid). All you have to do is type in your website’s domain / URL into the tool.

The other option is to use openssl on your server to encode and hash the value yourself. First you need to find the certificate. Since I use Let’s Encrypt. Mine was in /etc/le and called private.pem. All you need to do from there is run the command below:

**openssl x509 -pubkey &lt;** private.pem **| openssl pkey -pubin -outform der | openssl dgst -sha256 -binary | base64**

Be sure to change private.pem to your filename, all else should stay the same. This won’t modify anything but will echo your new pin.

## Getting a backup pin

Backup pins are a little tricker to create. First off, the reason we need a backup pin is we are limited what’s accepted by the browser. It’s a good idea to give the browser more then a single option just in case the private key from the first pin gets compromised. This method will generate a new CSR and private key for the same TLS certificate. Here are the steps. It’s very similar to generating the first pin, you just need to create the csr and key first.

**Step 1**: Generate a new private key  
 **openssl genrsa -out** name.your.backup.key **4096**  
– 4096 is the bits used (I think), 2048 or 4096 is the only two values you should be using.

**Step 2**: Generate a CSR  
**openssl req -new -key name.your.backup.key -sha256 -out** name.your.backup.csr  
– There are fields you will be prompted to fill in, do so to the best of your ability.

**Step 3**: Generate the second pin  
**openssl x509 -pubkey &lt;** name.your.backup.csr **| openssl pkey -pubin -outform der | openssl dgst -sha256 -binary | base64**

So now you should have both pins\*, for future reference, I would recommend writing a bash script to run these commands and then echo the pin values in the format of the header so it’s possible to copy and paste the header into a configuration file. Can you think of a better way? \*This is also a good time to mention that you will have to redo this EVERY time you change your TLS certificates, if you’re being super-cyber-secure, that means every three months. That means the more automated, the better!

## Adding a HPKP header

Now, depending on your sever, there are a lot of different ways to add a header to your requests. If you’re using apache, add a line to the vhost file. If you’re using nginx, there’s a similar line you can add to the configuration. If you’re using a fancy load balancer that’s too expensive for a guy running a blog off a VM, then you can use a rule designed for that load balancer.

I’d be worried to tell you too much about my architecture but the response header already straight up tells you I run on an apache server… \[note to self: remove that next time I’m digging in header configs and then update this blog post\]. Since we are all caught up on my server’s inner workings. I’ll go through the steps to add this to your apache config.

Step1:  
sudo **a2enmod headers**  
– You need to have permission to modify headers from the vhost file

Step 2:  
sudo **nano**/vim/vi \[your favorite text editor\] **server.conf** – Edit your vhost file and add the line in step 3

Step 3:  
**Header always set Public-Key-Pins “pin-sha256=\\”**base64+primary==**\\”; pin-sha256=\\”**base64+backup==**\\”; max-age=5184000; includeSubDomains”** – Save the vhost file

Step 4:  
sudo **service apache2 restart**

## Check your HPKP header is there

Your header should be there, if everything works well, your browser won’t yell at you about your website. Nothing will change, yay you’re done! But wait, how do we know? Well.. seeing that the pin is there is easy, check your request headers and you can see the pin. Or use another [report-uri tool](https://report-uri.io/home/pkp_analyse) that tells if the hash is valid and that you have your backup pin available.

<figure aria-describedby="caption-attachment-302" class="wp-caption aligncenter" id="attachment_302" style="width: 570px">[![HPKP header results](https://hackerunder.dev/wp-content/uploads/2016/04/Header-1.png)](https://hackerunder.dev/wp-content/uploads/2016/04/Header-1.png)<figcaption class="wp-caption-text" id="caption-attachment-302">Results from tool after HPKP header has been added</figcaption></figure>

## Final Comments

In a future blog post, I’ll be discussing a way to actually test the pins and see if the browser will respond well. Right now, I’ve only read about a method that requires purchasing a second certificate. To avoid that, I’m looking into any other possibilities such as using a MITM technique. The issue with that is modern browsers are doing a similar check even without the pins, so at this point in time I’m unsure if it’s the default browsers functionality or the new HPKP header.

If you have any issues with the steps, please comment below. Let me know if you want to see more about securing HTTPS protocols!

Special shout out again to <https://scotthelme.co.uk/hpkp-http-public-key-pinning/>. Thanks to Scott’s blog post’s I found the report-uri tools and was able to double check the commands I found on [Mozilla ](https://developer.mozilla.org/en-US/docs/Web/Security/Public_Key_Pinning)about getting a pin for the HPKP header.