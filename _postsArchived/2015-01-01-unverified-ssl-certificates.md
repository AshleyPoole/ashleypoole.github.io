---
layout: post
title: Unverified SSL Certificates (Web Security Series)
date: 2015-01-01 19:38
categories: [security]
tags: [hacking, ssl]
excerpt_separator: <!--more-->
---
This blog article is the first of a short series I'm writing on web security focusing in particular on mobile applications. This series will show real world examples I have uncovered of how security hasn't been implemented correctly with references to the <a title="Open Web Application Security Project" href="https://www.owasp.org/index.php/Main_Page" target="_blank">Open Web Application Security Project (OWASP)</a> where appropriate. My goal of this series is to highlight how not to implement web security to further help educate and highlight issues with security when it's not or incorrectly implemented.

<!--more-->

The topic for this blog article is on unverified SSL certificates which revolves around the verification process. Due to this being a heavy topic I will try not to dive too deeply but will instead give a high level overview demonstrating the process with example implementation flaws.

On a side note while I mention SSL in this article, I'm also talking about it's successor TLS though as most people tend to use two terms interchangeably I will be using the term SSL.
<h2>How SSL Certificates Are Verified</h2>
The basic process of verifying a certificate can easily be expressed as conversation between the client and the web server. A client can be anything from a web browser to an application making a HTTP request using it's native web request library or method.
<ul>
	<li><strong>Client:</strong> Hello there web server at https://www.ashleypoole.co.uk, I'm the Safari browser version 7 and would like to establish a secure connection with you. Can you identify yourself?</li>
	<li><strong>Web Server:</strong> Sure Safari browser version 7, here's my SSL certificate for https://www.ashleypoole.co.uk.</li>
	<li><strong>Client:</strong> Thanks web server, let me just check I trust that certificate from yourself.... <em>Computing, please wait... </em>Based on the signature and issuer of the certificate presented I trust you are indeed https://www.ashleypoole.co.uk and I'd like to continue with opening a secure connection with yourself.</li>
	<li><strong>Web Server:</strong> Sure thing, here's a signed acknowledgement from myself. Let's start chatting over this encrypted and secured channel about all that sensitive information you have for me!</li>
</ul>

Ok so I've over simplified the process a lot there but hopefully you get the picture. If the browser were to receive a certificate which isn't trusted or valid then the connection process would be terminated or a warning generated regarding the web servers identity couldn't be verified. Most browsers at this point will present an error asking if you'd really like to continue but warning you of the danger.

For example here is the warning message shown by Safari when the identify of a web server can not be verified. I personally believe this is one of the less scary and obstructive of warning messages compared to other browsers.

<a href="{{ "/img/2014/12/SafariSSLError.png" | prepend: site.assetsbaseurl }}"><img class="size-full wp-image-4491" src="{{ "/img/2014/12/SafariSSLError.png" | prepend: site.assetsbaseurl }}" alt="Safari SSL Error" width="625" height="198" /></a>  
[Safari Version 7.]

<h2>Why Not Verifying SSL Certificates Are (Very) Bad</h2>
Not verifying an SSL certificate means that the client cannot be sure that the web server responding to the requested domain (I.e https://ashleypoole.co.uk) is indeed the correct authoritative web server for the requested domain. This can lead to secure connections being created and sensitive data being transmitted to an unknown entity if the web servers identity hasn't been confirmed.

If a secure connection is created and the certificate isn't verified (also referred to as unvalidated certificate) this could unnecessary expose the secure connection between the client and web server to chances of interception which is known as a <a title="Man in the middle (MITM) attack" href="https://www.owasp.org/index.php/Man-in-the-middle_attack" target="_blank">man-in-the-middle (MITM) attack</a>.

<img class="" src="https://www.owasp.org/images/2/21/Main_the_middle.JPG" alt="MITM Attack" width="569" height="316" />  
[MITM Attack - https://www.owasp.org/index.php/Man-in-the-middle_attack]

If a MITM attack were to be performed this would then lead to the sensitive data from both the client and web server being exposed to a dangerous attacker. This sensitive data could be anything from payment details to identity information or government secrets to your weekly grocery shopping cart.
<h2>Mobile Applications Which Didn't Verify SSL Certificates In The Wild</h2>
During my testing of various Android applications I discovered two popular applications that didn't correctly implement SSL certificate verification. I cannot announce one of the applications yet though I can share the other one with you which was the <a title="Google Play Store - Cineworld Application " href="https://play.google.com/store/apps/details?id=com.cineworld&amp;hl=en_GB" target="_blank">Cineworld application from the Google Play Store</a>.

I discovered the lack of SSL certificate verification in v2.0.3 of the Cineworld application though I believe this bug could of possibly been present for sometime although the release notes under the "What's New" section of the Google Play Store page are a little very gage. I also of course haven't seen the code base so I cannot confirm my guess.

As you can see in the screenshot below, the username and password could of been exposed and possibly the payment information if I'd have continued with the process.

<img class="size-full wp-image-4521" src="{{ "/img/2015/01/CineworldAppInFiddler.png" | prepend: site.assetsbaseurl }}" alt="Cineworld App Fiddler Trace" width="1020" height="585" />  
[Fiddler trace of the Cineworld v2.0.3 Android application showing username and password as being visible.]

This security vulnerability / mis-implementation was discovered by simply proxying my device's through Fiddler and watching the traffic going back and fourth between the client, Fiddler and the web server. If you want to test out other applications on your own mobile device or are curious how to do this then head over to Telerik's documentation website where they have a document on <a title="Configuring Fiddler For Devices" href="http://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler" target="_blank">configuring Fiddler with mobile devices</a>.

If you wish to test for unverified SSL certificates it is import you do <strong>NOT</strong> install the Fiddler root certificate on your mobile device.
<h2>Summary</h2>
As you have hopefully learnt sensitive information can be exposed if SSL certificate verification isn't implemented.
<h2>Challenge</h2>
Below is a link to the saved Fiddler trace file for the Cineworld Android application v2.0.3 that I captured and use to generated the previous screenshot highlighting the username and password entered. Within the trace file there is another vulnerability from <a title="OWASP Top 10 Risks" href="https://www.owasp.org/index.php/Top_10_2013-Top_10" target="_blank">OWASP's Top 10 Risks</a> that could be potentially exploited.

<a title="Cineworld Fiddler Trace" href="http://goo.gl/SPcBJO" target="_blank">For the Cineworld Fiddler trace download click this text</a>.

If you can spot it leave a comment below and share the article!
