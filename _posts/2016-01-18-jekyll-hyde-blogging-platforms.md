---
layout: post
title: Jekyll & Hyde... Blogging Platforms
date: 2016-01-19 21:09
categories: [general]
---

Just like in the story of Jekyll & Hyde, my usage of Wordpress as my blogging platform has reached a bitter end. Over the last year, I found myself spending more time **trying** to write blog posts but fighting various WordPress issues at every turn. These issues included:  
- Database performance issues; thanks ClearDB! :(  
- SSL / authentication cookie issues  
- WordPress's funky newline handling  

Finally fed up of this and feeling enthused after attending <a href="http://ndc-london.com/" target="_blank">NDC London 2016</a> last week, I took the plunge and spent my weekend researching new blogging platforms. I had two requirements that my new blogging platform of choice must fulfill which was being performant, as well as being easy to publish new posts. I wanted a simplified workflow that allowed me to focus on writing blog posts, not maintaining the blogging platform itself as i'd previously been doing with WordPress!

After doing research into different blogging platforms, I decided to go for <a href="https://jekyllrb.com/">Jekyll</a> which is technically a static site generator rather than a blogging platform although it is blog-aware. Jekyll also runs natively on <a href="https://pages.github.com/">GitHub Page</a> which means I can reduce my hosting costs to *practiclly* nothing which of course is welcomed even if not a core requirement.

<a href="{{ "/img/2016/01/why-wordpress.png" | prepend: site.assetsbaseurl }}"><img src="{{ "/img/2016/01/why-wordpress.png" | prepend: site.assetsbaseurl }}" alt="why-wordpress" width="448" height="236" style="margin:0px auto;display:block"/></a>  
<center>Looking back now, I find myself asking why I stayed on WordPress for so long!</center>  


### Implementation
While I won't go into the full details here of how my hosting stack is now working (as I'm planning another post in the future on that), I did however want to call out all the components that I'm now using and brief note as to why.

These components are:  
* GitHub Pages for FREE hosting and generation of my Jekyll website.  
* Azure Blob Storage for hosting of my image assets.\*  
* Azure CDN for disturbing my image assets nearer to the readers of my website to help with page load time.  
* CouldFlare for CDN capabilities (even though GitHub pages provides this natively) but also mainly for their free universal SSL offering.\*\*  
* CDNJS for CDN capabilities of certain CSS and JS libraries.  

\* I decided not to host my image assets within the websites repository as there is a upper limit of 1GB per repository on GitHub. This also means that why Azure loses my website hosting business, they still get my storage business (fee's) though technically that comes out of my free Azure MSDN credits anyway :).

\*\* And yes, I know CloudFlare's universal SSL is *flawed* or well, not appropriate for some use cases as I've <a href="{% post_url 2014-12-12-cloudflares-universal-ssl-is-flawed %}/" target="_blank">already blogged about it</a>!

I'll go into more detail in the future and why I'm using multiple CDN's but for now, let's look at the outcome and benefits I've gotten from my migration to Jekyll.

### Outcome
I'm happy to say I now have a much simplified workflow and are able to publish new posts with ease, so easy in fact that all it takes is a **GIT PUSH** to my repository and BAM!, my new post is live. All this is thanks to GitHub Pages and their Jekyll support which if you're curious to know more, then you should head over to the <a href="https://help.github.com/articles/using-jekyll-with-pages/" target="_blank"> GitHub Pages documentation for Jekyll</a>.

Now the other important factor for me which was the need for speed. I read online that nearly half of web users expect a website to load within two seconds and also that they will likely abandon the site if it hasn't loaded within three seconds. With this in mind, I loaded the same article from both my WordPress and Jekyll implementations and surprisingly the results were as expected, with Jekyll (the static HTML site) providing greater performance over WordPress.

<a href="{{ "/img/2016/01/blog-comparison-speed-test.gif" | prepend: site.assetsbaseurl }}"><img src="{{ "/img/2016/01/blog-comparison-speed-test.gif" | prepend: site.assetsbaseurl }}" alt="website-speed-comparision" width="571" height="239" style="margin:0px auto;display:block"/></a>
<center>[Top test is WordPress with the Jekyll instance below it. Speed test has run using the <a href="http://tools.pingdom.com/fpt/">Pingdom</a> service.]</center>  
It's worthing noting I was also surprised by the performance experienced during the load time testing of WordPress, as it was some of the best I'd seen in a while from my instance. Maybe it knew I was about to replace it? ;-)

Based on the results, you can clearly see I have been able to get get the page load time down from > 2000ms to ~500ms which is a ~75% decrease. I've also been able to cut the page the page size down to 780KB from 1.1MB which gives me a ~29% decrease is page size.

With both of those improvements, I'm now once again a happy blogger and hopefully people who read my ramblings will now be able to do so with greater speed :-)

**\*\* UPDATE \*\***  
After further refactoring by using minified versions of JS / CSS as well as other features of CloudFlare, I've been able to get the page size down further to 616KB. This now gives me a ~44% decrease in page size and pushes my perf grade on Pingdom to 90/100.  
