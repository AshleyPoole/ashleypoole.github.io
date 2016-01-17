---
layout: post
title: MSCRM Update User From Database
date: 2013-08-19 17:56
categories: [knowledge-base, devops]
tags: [sql-server]
---
While updating the CRM database directly isn't support sometimes it is the only way to work around the restrictions of the WUI. An example of when it is needed, is when there are duplicate users and CRM got confused.

{% highlight sql %}
&lt;br /&gt;update SystemUserBase&lt;br /&gt;set FullName = 'New FullName'&lt;br /&gt;where FullName = 'Old FullName&lt;br /&gt; 
&lt;br /&gt;--check queue name as well&lt;br /&gt;select * from QueueBase&lt;br /&gt;where name like '%name%'&lt;br /&gt;
{% endhighlight %}
