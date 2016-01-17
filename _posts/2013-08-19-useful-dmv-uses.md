---
layout: post
title: Useful DMV Uses
date: 2013-08-19 18:05
categories: [databases, knowledge-base]
tags: [sql-server]
---
Shows connections as well as the login name. A Where clause can of course be added to filter to a certain username or client (login_name or client_net_address) if required.

{% highlight sql %}
select
	s.login_name
	,c.client_net_address
	,s.status
	,c.last_read
	,c.last_write
	,s.program_name
	,h.text last_query
from sys.dm_exec_connections c
join sys.dm_exec_sessions s
on c.session_id = s.session_id
cross apply sys.dm_exec_sql_text(c.most_recent_sql_handle) as h
{% endhighlight %}
