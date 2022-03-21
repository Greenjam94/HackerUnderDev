---
author: Greenjam94
categories:
- Uncategorized
date: "2016-04-22T07:00:44Z"
guid: https://hackerunder.dev/?p=321
id: 321
tags:
- blog.greenjam94.me import
title: Testing HPKP Headers
url: /testing-hpkp-headers/
---

Over the last two weeks, I’ve posting a lot about HTTP Public Key Pinning. This will be my last post about it, I want to focus on testing HPKP. If you don’t know what HPKP is, read the first post. To learn how to add those headers, read the second post.

I’ve had to spend a lot of time trying to figure out how to properly test these headers. In theory, this is how it should work. The first time your browser loads a website with HPKP, it saves the pin in it’s memory and then compares the pin on every request after that until the pin expires. If the browser ever finds a request without a pin or a incorrect pin, then the browser will block the request and warn the user.

## Failed attempts at testing HPKP

In order to test this, you should just have to change the pin. Easy right? Well so far the execution of that has been tricky. When I tried to change the pin to a random number that doesn’t match the cert, the browser seems to ignore the pin (According to my own results) and doesn’t send an error.

The next idea I attempted was to use [OWASP ZAP ](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)to [man-in-the-middle (MITM)](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) the request, which would change the certificate and the pin could be modified in all the requests. This also didn’t work because HPKP is ignored when a client trusted certificate is used instead of one approved by a CA. “Firefox (and Chrome) disable Pin Validation for Pinned Hosts whose validated certificate chain terminates at a user-defined trust anchor (rather than a built-in trust anchor).” –[Mozilla](https://developer.mozilla.org/en-US/docs/Web/Security/Public_Key_Pinning).

## Better ways for testing HPKP

What did seem to work was setting Chrome’s expected value to a false pin, and then getting an error when going to the site with a real pin. Here are the steps:

- Go to chrome://net-internals/#hsts in chrome
- Add your domain and use the example **sha256/7HIpactkIAq2Y49orFOOQKurWxmmSFZhBCoQYcRhJ3Y=** as the Public key fingerprints
- Go to your domain
- You should see an error message like the one below.

<figure aria-describedby="caption-attachment-322" class="wp-caption aligncenter" id="attachment_322" style="width: 595px">[![Chrome error message from testing HPKP](https://hackerunder.dev/wp-content/uploads/2016/04/Screen-Shot-2016-04-19-at-1.33.19-PM.png)](https://hackerunder.dev/wp-content/uploads/2016/04/Screen-Shot-2016-04-19-at-1.33.19-PM.png)<figcaption class="wp-caption-text" id="caption-attachment-322">Error message from testing HPKP</figcaption></figure>

This works for testing it from one application. What about other browsers like Firefox?

Another idea for testing HPKP is to keep the pin but change the certificate used to one that doesn’t match the pin. This requires buying another certificate because it has to be CA approved. You can either modify your domain’s headers to change the cert or you can try to use MITM and the CA cert instead of the self-signed cert.

Let me know how you test HPKP, it’s tricky business and isn’t easy to do. There are online tools like [report-uri’s HPKP analyser](https://report-uri.io/home/pkp_analyse) that can compare the certificate and the pin, but will not test the browsers usage of the pin. A lot of people think it’s good enough, but that’s not true. If browsers do not properly block other requests without a correct pin, then there is no point in using HPKP.