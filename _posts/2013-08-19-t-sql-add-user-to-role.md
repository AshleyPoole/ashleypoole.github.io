---
layout: post
title: T-SQL Add User To Role
date: 2013-08-19 17:57
author: ashley@ashleypoole.co.uk
comments: true
categories: [databases, knowledge-base]
---
SQL below to add a role to a user.

{% highlight sql %}
use MyDatabase;
go

sp_addrolemember 'db_owner', 'domainmyuser' ;
go
{% endhighlight %}
