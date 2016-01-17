---
layout: post
title: Reflection&#58; Public Speaking
date: 2015-08-30 22:42
categories: [general, security]
tags: [speaking]
---
Well I've been back in the UK for about a week now after attending an internal work conference in Minnesota, USA and wanted to reflect on my experience of public speaking while out there. At the conference I presented two sessions on information security, each lasting 45 minutes long and being delivered live to approximately 50+ software engineers. The titles of these sessions were:
<ul>
	<li>Secure Development Practices: For the modern day web developer</li>
	<li>Exploring The Dark Side Of Mobile Applications</li>
</ul>
Now at this stage I want to mention that I’m by no means a security expert by any stretch of the imagination, simply someone who has a growing interesting in the subject and likes ‘breaking’ things to understand how they work. (I also happen to follow some pretty smart people on social media including <a href="http://www.troyhunt.com/" target="_blank">Troy Hunt</a>, who I’m very much looking forward to attending his sessions at <a href="http://ndc-london.com/" target="_blank">NDC London 2016</a> conference).

While I cannot share the recording of sessions I did however want to briefly share what was covered in those sessions including the session feedback of those sessions which is the reasoning behind this post, as I believe it’s extremely important to take all feedback on-board and learn from it. Simply, it’s just another form of a peer review, just like when we’re writing code and create code review or pull request.

So, onto the first session…
<h2>Secure Development Practices: For the modern day web developer</h2>
In this session I paired with another software engineer from our HQ office in American to jointly present this session which looked at the top 10 vulnerabilities for web applications published by <a href="https://www.owasp.org/index.php/Top_10_2013-Top_10" target="_blank">OWASP</a>, the <a href="https://www.owasp.org/index.php/Top_10_2013-Top_10" target="_blank">Open Web Application Security Project</a>.

As each of the sessions were only 45 minutes long, we selected 6 of the 10 items of the OWASP vulnerability list to demonstrate with those being:
<ul>
	<li>A1 – Injection (SQL)</li>
	<li>A2 – Broken Authentication and Session Management</li>
	<li>A3 – Cross-site scripting (XSS)</li>
	<li>A5 – Security Misconfiguration</li>
	<li>A6 – Sensitive Data Exposure</li>
	<li>A10 – Unvalidated Redirects and Forwards</li>
</ul>
As we found out, covering all 6 of those vulnerable areas including demonstrations provided impossible and we simply ran out of time, leaving us to quickly rush through two of the vulnerable areas without any demonstrations.

The demo website we built can be found running at <a href="https://supersecure.website" target="_blank">https://supersecure.website</a> or the source code can be found on <a href="https://github.com/AshleyPoole/OWASP-2013-Demo" target="_blank">GitHub</a> (At time of writing this post, the source code we used is in the branch called GTSX).


<h3>Session Feedback: So how did we do?</h3>
Below are the statistics pulled from our session feedback. Both the number of voters and percentage are specified.

<a href="{{ "/img/2015/08/securedevelopment-feedback.png" | prepend: site.assetsbaseurl }}"><img src="{{ "/img/2015/08/securedevelopment-feedback.png" | prepend: site.assetsbaseurl }}" alt="Capture64-ddbc_checkdb_options" width="448" height="236" class="alignnone size-full"/></a>

Based on the session feedback, I’d personally say over all we did pretty well in the relevantly short space of time we had to present this topic. Also looking at the additional feedback section of the survey we did pretty well there also, with only a handful of comments regarding some technically difficulties experienced with the web proxy blocking our demo website; though I managed to recovery quickly and swap to a local hosted version of the demo website.

*Unbeknown to me though at this stage, this wasn’t going to be the first time our corporate web proxy would come to bite me in the ass as I found out in my second session 15 minutes later.*

<h2>Exploring The Dark Side Of Mobile Applications</h2>
This session focused not on coding and negating vulnerabilities unlike the previous session but instead on using real world examples of vulnerable mobile applications to demonstrate just how easy our sensitive data can be exposed. Usually this would all occur without our knowledge for the average user of a mobile application unless we peaked under the covers so to speak.

In this talk I planned on using three mobile applications (Android) that each previously contained vulnerabilities, with these being Nandos, <a href="http://www.ashleypoole.co.uk/2015/sensitive-data-exposure-pizza-express/" target="_blank">Pizza Express</a> and <a href="http://www.ashleypoole.co.uk/2015/unverified-ssl-certificates/" target="_blank">Cineworld</a>. The later two mobile applications both being one’s I had previously blogged about before, with all being discovered (and sent to the application developers) around the tail end of last year.

So back to that web proxy that I mentioned earlier. When I gave this session the web proxy blocked both the Pizza Express and Cineworld API calls due to their domain categories being blocked as per the corporate policy. After being able to successfully demo the Nandos application first, I had to instantly create a new approach to the session I was delivering and even against my best efforts, I think it of course showed.

I’m maybe being a bit too harsh on myself as having 2/3 of demonstration material wiped out isn’t an easy thing to deal with during a session. Ultimately I think I should have tested that all my demonstrations worked on the network that I was going to be using for the session ahead of time as I’d only tested them at home before my travels. This is certainly something I will keep in mind for any future talks I may give.

<h3>Session Feedback: So how did I do?</h3>
Like the previous session, below are the statistics pulled from my session feedback. Both the number of voters and percentage are specified.

<a href="{{ "/img/2015/08/darkside-feedback.png" | prepend: site.assetsbaseurl }}"><img src="{{ "/img/2015/08/darkside-feedback.png" | prepend: site.assetsbaseurl }}" alt="Capture64-ddbc_checkdb_options" width="448" height="236" class="alignnone size-full"/></a>

Based on the feedback statistics for this session I think the audience were kind in their grading's of my session and obviously were very accepting to my change of approach to the session caused by my limited demonstration material.

I did get a surprise though when I reviewed the additional comments for the session feedback. In fact I’d say I was blown away from the number of positive or supporting comments I received despite my demonstration failings. I think this clearly goes to show I work with a bunch of awesome people and maybe I recovered better than I thought (?).

Here’s a sample of the comments left, both the positive and negative:
<ul>
	<li>“The talk raised an important point about mobile security that's often forgotten”</li>
	<li>“Had trouble understanding Ashley”</li>
	<li>“Even though there were technical issues I still felt it was a great presentation. That's what happens sometimes with technology :)”</li>
	<li>“Very interesting and well done!”</li>
	<li>“Better set-up to anticipate technical difficulties would have been good”</li>
</ul>
<h2>What will I take away?</h2>
So after reading through all the feedback from both sessions, here’s the list of items I’m personally going to take away and work on for any future presenting opportunities:
<ul>
	<li>Live demo’s will never go right 100% of the time but in the future I should maximise my chances by testing out the network connection I will be using for my session ahead of time.</li>
	<li>Speak clearer - This will most likely be the hardest item to work on but will hopefully be something that I can improve over time in a continuous improvement type pattern.</li>
	<li>Don’t be so self critical.</li>
</ul>
