---
layout: post
title: Why I Moved To Azure
date: 2014-11-30 20:40
categories: [general]
tags: [azure]
---
With 2015 fast approaching and a few hours spare, I decided it was time to bite the bullet and migrate most of my websites that I own (including this blog) to Azure. Now I want to point out this wasn't my first experience of using Azure but it was however the first time I'd forced myself to use it full time and for my live websites.

Until now I had been using a Linux based Virtual Private Server (VPS) with only 128MB of RAM, running up to 5 websites at times of which four of them were Wordpress installations. I also hosted the MySQL databases for those WordPress installations on the same server, all for only $17.99 which works out to about £11.50 - £12 for a year.

As you'll likely tell, I had over provisioned the server though due to some careful memory management on my part and CloudFlare's free CDN package, all my sites were responsive and loaded within an acceptable time frame. Now I don't intend to drop this VPS now that I have completed my migration though I will repurpose it to backup my Azure websites and resources.

Now this blog post isn't about the technical details rather what my motivation was behind the migration. This leads me onto the two key reasons behind the migration which were:
<ol>
	<li>New Technology - I love exploring new technologies and while Azure isn't that new now, it was however a mostly unexplored technology for myself. Therefore is was logical that I should take the first step and move my websites over to the platform to encourage my use and learning.</li>
	<li>Azure Credit With MSDN Subscription - With an MSDN subscription, you have a set amount of included Azure credits every month which ranges between £35 and £95 depending on your subscription level. This included allocation meant I could explorer the platform without having to splash out any of my own cash! Win, Win.</li>
</ol>
I must say my experience so far has been very positive and I will continue to explorer and develop against the platform. If you have an MSDN subscription I fully recommend you checkout the following link to see what your included Azure credits allocation are and how to get started - <a title="Azure Credits Included With MSDN Subcription" href="http://azure.microsoft.com/en-gb/pricing/member-offers/msdn-benefits-details/" target="_blank">http://azure.microsoft.com/en-gb/pricing/member-offers/msdn-benefits-details/</a>.

Hopefully none of my readers will notice any difference except for the improved request and load times as well as the fact I have also swapped to using FeedBurner by Google for my <a title="My RSS Feed" href="http://feeds.feedburner.com/AshleyPoole" target="_blank">blogs RSS feed</a>. I have done this as it allows me to optimise usage of my Azure resources by reducing requests directly against my website in Azure.
