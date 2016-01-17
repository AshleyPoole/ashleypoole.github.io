---
layout: post
title: Introducing SSLLWrapper
date: 2015-01-20 22:28
categories: [programming, security]
tags: [powershell]
---
<h2>What Is SSLLWrapper</h2>
<blockquote>SSLLWrapper stands for SSL Labs Wrapper which is the first publicly available .NET wrapper developed for the <a title="SSL Labs Assessment API's GitHub" href="https://github.com/ssllabs/ssllabs-scan/blob/master/ssllabs-api-docs.md" target="_blank">SSL Labs' Assessment API's</a> that allow the consumer to test SSL servers on the public internet.

This wrapper easies the communication to the API's for .NET developers which allows you as the developer to focus on your project rather than managing the plumbing and overhead required to consume the API's.

( Quoted from <a title="ssllwrapper page" href="/ssllwrapper/" target="_blank"><strong>ssllwrapper documentation</strong></a> )</blockquote>
<h2>Why I Wrote SSLLWrapper</h2>
While performing research for an upcoming project I came across the public Qualys SSL Labs' Assessment API's which are currently in development. These API's seemed to fit my need exactly but from my research I was unable to find a .NET wrapper that was available.

Therefore I decided to set about developing my own .NET wrapper which I wanted to release to the community.
<h2>How Can You Consume SSLLWrapper</h2>
The wrapper can easily be added to your project by using NuGet. Either search for "SSLLabsApiWrapper" through the NuGet Visual Studio packages dialog or by typing "<em>Install-Package SSLLabsApiWrapper</em>" into the NuGet package manager console.<!--more-->

Once the package has been added to your project, you can then simply import the "SSLLWrapper" namespace and create a new instance of the SSLLService class passing in the SSL Labs Assessment API url. You will then be able to access the API methods following the SSL Labs documentation.

Example accessing Analyze API method:

{% highlight c# %}

var ssllService = new SSLLService("https://api.ssllabs.com/api/v2");  
var response = ssllService.Analyze("https://www.ashleypoole.co.uk");

{% endhighlight %}


Hopefully this short introduction has given you the basic information regarding what the wrapper is about and also how to consume it. For more detailed information on consumption and responses, please check out the <a title="ssllwrapper project page" href="http://www.ashleypoole.co.uk/ssllabs-api-wrapper" target="_blank">projects page</a> or alternatively if you'd like to contribute and/or look through the source code then check out the <a title="SSLLWrapper GitHub" href="https://github.com/AshleyPoole/SSLLabs-Api-Wrapper" target="_blank">projects repo on GitHub</a>.

<strong>** Update **</strong>

Since writing this article I have renamed the project to SSL Labs Api Wrapper. Therefore the namespace and service names have changed. Please see the <a title="SSL Labs Api Wrapper" href="http://www.ashleypoole.co.uk/ssllabs-api-wrapper" target="_blank">projects home page </a>for changes.
