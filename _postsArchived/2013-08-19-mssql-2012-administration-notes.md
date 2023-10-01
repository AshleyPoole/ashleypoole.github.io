---
layout: post
title: MSSQL 2012 Administration Notes
date: 2013-08-19 18:06
categories: [databases, knowledge-base]
tags: [sql-server, notes]
---
Is the current SQL instance is part of a cluster?

{% highlight sql %}
SELECT SERVERPROPERTY('IsClustered');
{% endhighlight %}


Flush dirty pages to disk - What this does is determined by the Recovery mode set on the database. It can also truncate the trans log depending on the Recovery mode. Maybe something to only run in DEV.

{% highlight sql %}
CHECKPOINT;
GO
{% endhighlight %}


Clear out cache

{% highlight sql %}
DBCC DROPCLEANBUFFERS;
DBCC FreeProcCache
{% endhighlight %}


Show the query plan without using the UI

{% highlight sql %}
SET SHOWPLAN_TEXT ON;
{% endhighlight %}


Sets the statistics for Time and IO on for the session

{% highlight sql %}
SET STATISTICS TIME ON;
SET STATISTICS IO ON;
{% endhighlight %}


Dynamic Management Views (DMV) - Returns server state information
CHECKPOINT Internval

{% highlight sql %}
sp_configure
{% endhighlight %}


Then change RecoveryInternval - Default every minute
