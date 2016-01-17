---
layout: post
title: Broken Microsoft Exception Link
date: 2013-08-09 21:17
categories: [general]
tags: [rant]
---
The moment when your wcf service throws an exception and the exception help link is broken as well... Sigh.

In this case, the exception being thrown was:
"HTTP could not register URL http://+:8001/. Your process does not have access rights to this namespace (see <a href="http://go.microsoft.com/fwlink/?linkid=70353" target="_blank">http://go.microsoft.com/fwlink/?linkid=70353</a> for details".

When trying to go to the above go.microsoft.com link, you are attempted to be redirected to the following broken URL for a MSDN page on the Windows SDK <a href="http://windowssdk.msdn.microsoft.com/en-us/library/ms733768.aspx" target="_blank">http://windowssdk.msdn.microsoft.com/en-us/library/ms733768.aspx</a>.

Long story short, I believe the page should be linking to the following page on <a href="http://msdn.microsoft.com/en-us/library/ms733768.aspx" target="_blank">http://msdn.microsoft.com/en-us/library/ms733768.aspx</a>.

During trying to resolve this issue, the tool called HttpNamespaceManager was discovered that is written by an Microsoft employee and though while not an official Microsoft tool, it does provide a great way to manage namespace reservations. More on HttpNamespaceManager and the download link can be found at <a href="http://blogs.msdn.com/b/paulwh/archive/2007/05/04/addressaccessdeniedexception-http-could-not-register-url-http-8080.aspx" target="_blank">http://blogs.msdn.com/b/paulwh/archive/2007/05/04/addressaccessdeniedexception-http-could-not-register-url-http-8080.aspx</a>.

Am I really asking a lot for Microsoft to keep their exception helper links up to date?
