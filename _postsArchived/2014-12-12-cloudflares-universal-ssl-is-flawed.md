---
layout: post
title: Why I Think CloudFlare's Universal SSL Is Flawed
date: 2014-12-12 16:48
categories: [security]
tags: [cloudflare, ssl]
excerpt_separator: <!--more-->
---
Now I have to start this blog post stating that I love every bit of <a title="CloudFlare Home Page" href="https://www.cloudflare.com/" target="_blank">CloudFlare</a>, their offerings and what their about. Hell, you could even call me one of their <a title="Fanboy Wikipedia" href="http://en.wikipedia.org/wiki/Fan_(person)" target="_blank">fanboys</a> as I rave about them at just about every opportunity and I even use their services for my blog that you're reading now!

excerpt_separator: <!--more-->

## What is Universal SSL

CloudFlare recently announced and implemented a feature called<a title="CloudFlare Universal SSL" href="https://www.cloudflare.com/ssl" target="_blank"> Universal SSL </a>which in their words is about:
<blockquote>"encrypting as much web traffic as possible to prevent data theft and other tampering". Also continuing on to state they are "proud to be the first Internet performance and security company to offer SSL protection at no cost".</blockquote>
This announcement was no small feat towards securing more of our web traffic and one that should certainly be applauded. However this is unfortunately where my love of Universal SSL becomes divided and in the case of the Flexible SSL and Full SSL implementations turns into absolute horror.

Now some of you will have just read that last paragraph and be asking yourselves why because surely increasing the use of SSL / TLS to encrypt more of our web traffic can only be a good thing in this day and age? The short answer is yes, increasing the usage of <strong>correctly</strong> implemented SSL is an extremely positive step forward though when implemented incorrectly this can be as <strong>dangerous</strong> as not running SSL in my opinion. In fact, I'd say it wouldn't be a stretch to say worse as you and your visitors are given a false sense of security.

Every time I come across an incorrect SSL implementation in the wild my <a title="Heartbleed bug" href="http://heartbleed.com/" target="_blank">Heartbleeds</a>.

<img class="" src="http://heartbleed.com/heartbleed.png" alt="Heartbleed logo" width="341" height="413" />  
[Heartbleed logo. Source: http://heartbleed.com/heartbleed.png]

<h2>Why I think two of the Universal SSL implementations are flawed?</h2>
I'm going to start off my thoughts of why Universal SSL is flawed by sharing this diagram from CloudFlare's website which highlights the three different modes that Universal SSL can operated in. Pay carefully attention to the Flexible and Full SSL mode's in the diagram.

<img class="wp-image-3871 size-full" src="{{ "/img/2014/12/ssl-cloudflare.png" | prepend: site.assetsbaseurl }}" alt="SSL CloudFlare Diagram" width="708" height="569" />  
[Universal SSL Diagram - https://support.cloudflare.com/hc/en-us/articles/200170416]

<h3>Flexible SSL</h3>
As you can see from the diagram above, in Flexible SSL mode an SSL connection is presented to the visitors browser though the backend connection from CloudFlare data centre to the origin server isn't over SSL. This falsely gives the impression to the client that they are operating over a secure connection and thus may be more likely to trust the website with more sensitive information compared to an unencrypted connection.

The hard truth is that the visitors sensitive data has in fact been sent over a non-encrypted connection from CloudFlare's data centre to the origin server, potentially exposing this sensitive information to man in the middle attacks all without the visitor and their browser none the wiser.

Reading CloudFlare's <a title="CloudFlare Universal SSL Support Page" href="https://support.cloudflare.com/hc/en-us/articles/200170416" target="_blank">support page</a> for Universal SSL which is quoted below, they even recommend against using Flexible SSL yet the option is still available and no warning is given to the web master when enabling the Flexible SSL option within the management portal.
<blockquote><strong>"NOTE</strong>: Flexible SSL is <span class="wysiwyg-underline"><strong>not</strong></span> recommended if you have any sensitive information on your website.  This option should only be used as a last resort if you are not able to setup SSL on your own web server. This option is far <span class="wysiwyg-underline">less secure</span> than the Full SSL option indicated below." (<a title="CloudFlare SSL Article" href="https://support.cloudflare.com/hc/en-us/articles/200170416" target="_blank">https://support.cloudflare.com/hc/en-us/articles/200170416</a>)</blockquote>
<h3>Full SSL</h3>
From the Universal SSL diagram you will also see that Full SSL ensures that traffic is encrypted from the browser right through to the origin server. These means that any sensitive data is always encrypted during transmission however the authenticity of the SSL certificate on the origin server ins't validated.

Not validating the authenticity of the certificate on the origin server means that while the traffic is encrypted a <a title="Man in the middle attack" href="https://www.owasp.org/index.php/Man-in-the-middle_attack" target="_blank">man in the middle (MITM) attack</a> is still possible. This is because an attacker could intercept traffic destined for origin server using a self signed or invalid certificate and the connection would still be established by CloudFlare. The attacker could therefore steal any encrypted data sent by the client and their browser to the origin server.
<h2>Summary</h2>
I personally advise that under no circumstances should the Flexible SSL option be used (yet alone exist) for the false sense of security it gives visitors - as their data will only be encrypted between the bowser and CloudFlare's data centre, not the origin server too.

The Full SSL option would be acceptable if CloudFlare were to implement and enforce <a title="certificate pinning" href="https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#What_Is_Pinning.3F" target="_blank">certificate pinning</a>. Certificate pinning would allow the connection between CloudFlare and the origin server to only be established if an expected certificate/key is presented by the origin server.


Update - As it turns out CloudFlare are already exploring the possibility of <a title="CloudFlare Implementing Certificate Pinning" href="http://blog.cloudflare.com/origin-server-connection-security-with-universal-ssl/" target="_blank">implementing certificate pinning</a> which I believe is a positive course of action.
