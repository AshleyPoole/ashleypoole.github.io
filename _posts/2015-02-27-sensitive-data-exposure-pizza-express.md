---
layout: post
title: Sensitive Data Exposure With Every Delicious Pizza
date: 2015-02-27 07:12
categories: [security]
tags: [hacking]
---
This blog article is on sensitive data exposure which is the second article of a short series I’m writing on web security, focusing particularly on mobile applications. This series will show real world examples I have uncovered of how security hasn’t been implemented correctly with references to the <a title="Open Web Application Security Project" href="https://www.owasp.org/index.php/Main_Page" target="_blank">Open Web Application Security Project (OWAP)</a> where appropriate. My goal of this series is to highlight how not to implement web security to further help educate and highlight issues where security is not or incorrectly implemented.
<h2>What Is Sensitive Data Exposure</h2>
Before defining sensitive data exposure, let's briefly explore the definition of sensitive data which is any data that requires extra protection that normally includes passwords, credit card numbers, health records and personal information.

OWASP defines sensitive data exposure as:
<blockquote>Many web applications do not properly protect sensitive data, such as credit cards, tax IDs, and authentication credentials. Attackers may steal or modify such weakly protected data to conduct credit card fraud, identity theft, or other crimes. Sensitive data deserves extra protection such as encryption at rest or in transit, as well as special precautions when exchanged with the browser.</blockquote>
OWASP also lists the risk of sensitive data exposure as follows:

<img class="aligncenter size-full wp-image-5141" src="{{ "/img/2015/02/owasp-sensitive-data-threat.png" | prepend: site.assetsbaseurl }}" alt="owasp sensitive data threat" width="800" height="371" />

<h2>How Can Sensitive Data Be Exposed</h2>
Sensitive data can be in a number of different ways though the most common are:

<strong>1. Sensitive data is stored in clear text</strong>

Sensitive data stored in clear text is extremely vulnerable to exposure whether that be within a database, data file or log file from either internal users or attackers.

You may have thought the log file wouldn't be an issue as you, the application developer are in control of what gets written to those logs but what about if you pass sensitive data as a query string parameter? Hello web logs that contain requests url query string!

<strong>2. Sensitive data is transmitted without encryption (clear text)</strong>

The internet is a dangerous place ripe with opportunities for attackers to perform man in the middle (MITM) attacks allowing them to record your traffic going back and fourth between the client and your web server. Now if any of that data is sensitive and not encrypted, you've basically just handed it to the attackers on a plate with a dine here for free sign hanging right above it!

As well as attackers however, companies or even governments may (they do!) record information that passes through their network whether this be on a LAN or conduits on the internet and use this information for whatever their needs maybe. Spying if your the NSA.

<strong>3. Sensitive data is transmitted with weak or old cryptographic algorithms</strong>

Encrypting sensitive data during transit with weak or old <a title="Cryptography" href="https://www.owasp.org/index.php/Guide_to_Cryptography" target="_blank">cryptographic algorithms</a> could potentially put the data at risk as the cryptographic could have known issues or easily broken. For example it is said that the DES cryptographic algorithm can be broken (decrypted) by an average desktop PC. As well as DES, it is recommended to also avoid SHA-0, SHA-1 and now also MD5 though MD5 is still used by certain (primarily old) devices.

<strong>4. Sensitive data entered through a form is remembered by the browser (auto complete)</strong>

Sensitive data be be exposed from the browser if it hasn't been instructed to disable auto complete on a web form. Auto complete is a feature implemented in most browsers to remember information typed into web forms that enable the user to quickly re-enter the same information again. This feature can be useful though is extremely dangerous on shared PC or device as other users could be able to see auto complete information from a previous user.
<h2> Sensitive Data Exposure In The Wild</h2>
Last year during my testing of various Android applications I discovered that PizzaExpress's Android application was transmitting sensitive data over HTTP. Yes you read that correctly, your sensitive data including credit card information was being sent over HTTP for all to see!

Here's what one of those requests from the application looked like when running it through Fiddler and generating a trace file:

<a href="{{ "/img/2015/02/pizzaexpress-http-request.png" | prepend: site.assetsbaseurl }}"><img class="aligncenter size-full wp-image-5301" src="{{ "/img/2015/02/pizzaexpress-http-request.png" | prepend: site.assetsbaseurl }}" alt="PizzaExpress HTTP  Request" width="1900" height="1006" /></a>

As you can just about make out in the screenshot, sensitive payment information like the credit card number (cc_number parameter) and the credit card cvv number (cc_cvv parameter) are visible as they have been sent over the wire in clear text.

The same vulnerability also existed for the customer's login and registration pages which posted the users credentials over the wire in clear text too. Therefore if you've ever used the PizzaExpress mobile application and have registered, I <strong>strongly</strong> recommend you to change your password as a matter of precaution.

[caption id="attachment_5401" align="alignnone" width="800"]<a href="{{ "/img/2015/02/izzaexpress-post-password.png" | prepend: site.assetsbaseurl }}"><img class="wp-image-5401 size-full" src="{{ "/img/2015/02/izzaexpress-post-password.png" | prepend: site.assetsbaseurl }}" alt="PizzaExpress Posts Clear Text Passwords" width="800" height="352" /></a> Screenshot of Fiddler session showing email address and password being POSTed over HTTP.[/caption]

After making the discovery I promptly informed PizzaExpress detailing my findings and received the following reply from them a couple of weeks after reaching out to them.
<blockquote>
<p class="p1">Thank you for raising the issues that you found with our Mobile App.</p>
<p class="p1">We can confirm that we are aware of both of these issues and we have been working with our third party developers on a fix. This is currently undergoing UAT (User Acceptance Testing)  – prior to us authorising for release to Google Play &amp; the App Store.</p>
<p class="p1">The updated apps will be in their respective app stores shortly.</p>
</blockquote>
<h2>Close but no cigar</h2>
After I got confirmation that the PizzaExpress application was no longer exposing sensitive data I decided it was time to publicly publish this article about my white hat hacker style discovery. This is what then lead me to retest their application where I discovered they had indeed resolved the issue of sensitive information being sent over the wire in clear text but (there's always a but) hadn't quite closed the loop hole to prevent a different variation of this vulnerability from happening again.

So what is the issue now?

The issue is that the application is still vulnerable to leaking sensitive data if intercepted by a <a title="MITM Attack" href="https://www.owasp.org/index.php/Man-in-the-middle_attack" target="_blank">MITM</a> attack due to how they have implemented their application settings loading process when the application starts up.

When the application first opens up it makes a HTTP request to <a title="pizzaexpress application settings" href="http://pizzaexpress.mweb.qa.demo.2ergo.com/config_json.php?appshell_id=android_1.0" target="_blank">http://pizzaexpress.mweb.qa.demo.2ergo.com/config_json.php?appshell_id=android_1.0</a> which servers up a JSON payload containing the application settings including the Api server's url. Therefore someone could easily rewrite the request protocol from HTTPS to HTTP and thus the sensitive data would be sent over clear text. Even worse would be that someone could also easily rewrite the entire Api url to be directed to another third parties web server which could harvest the sensitive data!
<blockquote>

{% highlight json %}
{
    "name":"live",
    "web_server":"https://pe-android.is.2ergo.com/",
    "api_server":"https://pizzaexpress.is.2ergo.com/"
}
{% endhighlight %}

This is a snippet of the JSON payload delivered by config_json.php</blockquote>

If you're interested in looking into how to rewrite the request protocol using Fiddler (which is very easy) then I recommend you checkout the <a title="FiddlerScript Editor" href="http://www.telerik.com/download/fiddler/fiddlerscript-editor" target="_blank">FiddlerScript Editor</a> add-in and also Telerik's knowledge base article on <a title="Modifying a request or response with fiddler script" href="http://docs.telerik.com/fiddler/KnowledgeBase/FiddlerScript/ModifyRequestOrResponse" target="_blank">modifying a request or response</a>.

Finally, another possible issue is that the web logs for the server farm serving up the <a title="pe-android.is.2ergo.com" href="http://pe-android.is.2ergo.com" target="_blank">pe-android.is.2ergo.com</a> Api endpoint could also be recording the sensitive data from the requests query string in it's web server logs. Theres unfortunately no way to confirm either way if this is happening but if it is then this means sensitive data might be stored on the <a title="pe-android.is.2ergo.com" href="http://pe-android.is.2ergo.com" target="_blank">pe-android.is.2ergo.com</a> web servers logs in clear text. This could be very dangerous and would unnecessary expose customers sensitive data from internal abuse as well as external attackers if those logs were ever stolen.

For example within IIS having <a title="enable cs-uri-query logging" href="http://stackoverflow.com/questions/19918561/does-iis7-log-request-query-string-by-default" target="_blank">cs-uri-query logging enabled</a> would mean that such query string parameters like the credit cards cvv number would be recorded. As we don't know the configuration of the <a title="pe-android.is.2ergo.com" href="http://pe-android.is.2ergo.com" target="_blank">pe-android.is.2ergo.com</a> server(s) this question will like go unanswered.

Again, if you've ever registered with PizzaExpress and used their mobile application I recommend you go and change your password as well as anywhere else you may have reused that same password.

If you're interested in obtaining a copy of the Fiddler trace then please contact me using any of my social media details in the blogs header including my <a title="Ashley Poole Twitter" href="https://twitter.com/AshleyPooleUK" target="_blank">Twitter</a>.
