---
layout: post
title: Introducing SSL Notifications (Beta)
date: 2015-06-28 19:32
categories: [security]
tags: [ssl]
excerpt_separator: <!--more-->
---
When dealing with HTTPS endpoints that I managed, I often found myself turning to online scan providers including <a href="https://www.ssllabs.com/ssltest/analyze.html" target="_blank">SSL Labs</a> to manually carry out testing of these endpoints. This required me to A), Remember to periodically scan those endpoints and B), Have the free time required to scan all the endpoints.

<!--more-->

With the release of the public <a href="https://github.com/ssllabs/ssllabs-scan/blob/stable/ssllabs-api-docs.md" target="_blank">API’s from SSL Labs</a> it sparked an idea within me and I set about creating a .NET wrapper* for the API which I released to <a href="https://github.com/AshleyPoole/ssllabs-api-wrapper" target="_blank">GitHub</a> as an open source project and also as a <a href="https://www.nuget.org/packages/SSLLabsapiwrapper" target="_blank">NuGet package</a> for ease of use.
<h2>Introducing SSL Notifications</h2>
The .NET wrapper was the first* stage in creating a free TLS/SSL notification service for your servers security status, which I’ve now released in beta called SSL Notifications and can be accessed via <a href="https://sslnotifications.com" target="_blank">https://sslnotifications.com</a>. (Yes, of course the service is using TLS… Thanks <a href="https://twitter.com/CloudFlare" target="_blank">@Cloudflare</a> and <a href="https://twitter.com/Azure" target="_blank">@Azure</a>).

SSL Notifications provides an automated way to periodically receive notifications regarding your TLS / SSL servers security status including vulnerability and certificate information. All this is available through a <a href="https://sslnotifications.com/signup" target="_blank">free subscription</a> which only requires a valid email address and the domain name of the endpoint you’d like scanning.<!--more-->

So what’s with the asterisk’s (*) next to mention of the API wrapper, good question. Sadly when I approached Qualys SSL Labs before launching the beta service, my usage of their API was rejected for two reasons:
<ol>
	<li>I apparently didn’t meet their <a href="https://www.ssllabs.com/about/terms.html" target="_blank">usage criteria</a> of the API.</li>
	<li>They also have tentative plans to build similar features directly into their offering and didn't want to support a competing offering.</li>
</ol>
Getting my request for usage of their API rejected was a huge shame and a knock back to my service, especially as I though this would be a great addition for the community. Nonetheless though, I have pushed forward and implemented another backend scan provider for the SSL Notifications service (though I have left the Qualys SSL Labs implementation there and ready to go, just in case they change their mind :) ).

I'll publish more information to my blog and the site at a latter date on the full list of items checked, but rest assured this includes vulnerabilities such as POODLE, Heartbleed and Logjam. Simply sit back and simply let the notifications come to you!
<h2>Why?</h2>
So why did I write SSL Notifications then? Well as I’ve briefly tried to explain, I wanted to resolve the two issues I was experiencing, which thinking back were remembering to scan my endpoints and also having the time to conduct those manual scans. So with my DevOps style hat on, I set about creating the service your reading about now, SSL Notifications.

Also I thought too, why only write the solution for myself, what about the community too?!

Another personal point to the project was that I wanted to stretch my feet a little with both Azure and new technologies that I hadn’t previously used myself in production applications, including Entity Framework to name only one. It's certainly been a learning curved that I've enjoyed!
<h2>Go Play!</h2>
The service is only in beta but I have many more features planned, so don’t take my word for it, go <a href="https://sslnotifications.com/Signup" target="_blank">sign-up</a> for your FREE subscription(s) now!

All feedback is welcome and encouraged, especially as I’m writing this service with the community in the forefront of my mind. Hope you enjoy!
