---
layout: post
title: MSCRM Performance Optimisation Using Compression
date: 2013-09-08 19:53
categories: [Knowledge Base, server-admin]
---
<strong>Server side for optimising the client</strong>

Compression within IIS can be configured to speed up clients. Both static and dynamic responses can be compressed.

By default MS CRM 2011 is configured to compress web responses that are sent to browser clients though is not default for SDK clients such as Microsoft Dynamics CRM for Outlook client. The reason this is not turned on by default is due to the setting is a server level settings and would affect all websites.

{% highlight powershell %}
-- Configure from console
-- An IIS reset is required after running the below in an administrative console.

%SYSTEMROOT%system32inetsrvappcmd.exe set config /section:system.webServer/httpCompression /+&quot;dynamicTypes.[mimeType='application/soap%%u002bxml; charset=utf-8',enabled='true']&quot; /commit:apphost

-- To turn it off
%SYSTEMROOT%system32inetsrvappcmd.exe set config - section:system.webServer/httpCompression/- &quot;dynamicTypes.[mimeType='application/soap%%u002bxml; charset=utf- 8',enabled='true']&quot; /commit:apphost
{% endhighlight %}

http://www.microsoft.com/en-gb/download/details.aspx?id=27139
