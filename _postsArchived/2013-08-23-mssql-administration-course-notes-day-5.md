---
layout: post
title: MSSQL Administration Course Notes – Day 5
date: 2013-08-23 20:12
categories: [databases, knowledge-base]
tags: [training]
---
###Yesterdays Review

A DDBC with data loss will actually just removes the corrupted data

You can only have one clustered index on a table. Normally on the primary key by default.

Show fragmentation - sys.dm_db_index_physical_stats
http://technet.microsoft.com/en-us/library/ms188917.aspx

Doing index rebuilding on maintenance plan is not the smartest thing to do as will blindly rebuild even if not needed. See link in previous days notes for a smarter indexing script.

Normally index columns frequently listed in WHERE clauses not the SELECT clauses.

Index ID = 0 is heap, index id 1 is clustered index

----------

###AM Session - Tracing and Tuning Advisor  

Classic - SQL Server Profiler
New - Extended Events

Get Profiler events as a table style - Setup profiler then use the below to view the results as a table. It's recommended to always write to file then convert.
SELECT * FROM fn_trace_gettable('C:test.trc', default)

Extended Events  
Server &gt; Management &gt; Extended Events &gt; Sessions &gt; Right Click - New Session Wizard

If wanting to profiler for longer than a few minutes, use Extended Events over SQL Profiler any day!

you can setup SQL Profiler, then export the configuration to SQL. When running the SQL, it will return a traceID. The below SQL will get all the traces. Your looking for property = 5 and the value column. If value is equal 1 then the trace is running.
SQL has a black box trace which is trace 1. The log will contain a trace should SQL crash.
SELECT * fn_trace_getinfo(default)

To turn the trace off
{% highlight sql %}
EXEC sp_trace_setstatus &amp;amp;lt;traceid&amp;amp;gt;, 0; -- Zero turns the trace off
EXEC sp_trace_setstatus &amp;amp;lt;traceid&amp;amp;gt;, 2; -- Removes the trace
{% endhighlight %}

Replaying a trace will of course redo all the events again. This isn't something you would want to reply on a production server.


Export a trace config for use in SSMS
File &gt; Export &gt; Script Trace Definition &gt; For SQL

Use tuning advisor, you need to use a real life example data set for testing, else it will optimise the indexes on just that data set you supply. Use tracing for a period of time and supply that to the tuning advisor as it's data/query set.

If log shipping, you have to create indexes on both servers as well as rebuilding them. I believe this will be the same for AlwaysOn too as they function very similar on the database level.

ColumnStore Indexes in SQL2012 made the columns read only though MS state that is could improve access speed up to 100 times. In SQL 2014, we should expect ColumnStore indexes to no longer required the columns be set to read only.


----------

###Dynamic Management Views

<a href="http://sqlserverperformance.wordpress.com/2011/02/04/five-dmv-queries-that-will-make-you-a-superhero-in-2011/">http://sqlserverperformance.wordpress.com/2011/02/04/five-dmv-queries-that-will-make-you-a-superhero-in-2011/</a>
<a href="http://sqlserverperformance.wordpress.com/2008/01/21/five-dmv-queries-that-will-make-you-a-superhero/">http://sqlserverperformance.wordpress.com/2008/01/21/five-dmv-queries-that-will-make-you-a-superhero/</a>



<a href="{{ "/img/2013/08/Capture74-DMV-CatsMini.png" | prepend: site.assetsbaseurl }}"><img src="{{ "/img/2013/08/Capture74-DMV-CatsMini.png" | prepend: site.assetsbaseurl }}" alt="Capture64-ddbc_checkdb_options" width="127" height="174" class="alignnone size-full" /></a>

More are available


<strong>Locking</strong>  
sp_lock - Mode Column - Old way, Cannot select against it
S = Shared Lock = Read
X = Exclusive = Write - No reads can take place. Can always change isolation level though to allow read even uncommited. If you were to read without the lock, the data would be dirty.
I = Intent - You will see those in front of other of mode types. This is a type of warning. An IX is always related/child of an X lock.
{% highlight sql %}
SELECT * FROM sys.dm_tran_locks
{% endhighlight %}


Read locked tables though will show dirty data which could be rolled back after read. Sometimes though in large systems, this is one of the only ways to run reports which a high data change rate.
{% highlight sql %}
SELECT * FROM mytable (NOLOCK)
{% endhighlight %}


Data collection  
Used basic activation of data collection. Data collections can be used to gather statistics from multiple servers / instances.
Read more from below link on custom collections.
<a href="http://technet.microsoft.com/en-us/library/bb677277.aspx">http://technet.microsoft.com/en-us/library/bb677277.aspx</a>
Setup a new DB for the warehouse on the first server instance (master), then configure a data collection on the second to point at the first. You can then Right Click on the DB Warehouse on the first instance, Management Data warehouse &gt; Overview


----------

###PM Session

Manage multiple servers - Really helpful  
View &gt; Registered Servers &gt; Create new Central Management Servers group. Then register server.

DAC Packs  
Create - Right Click DB &gt; Tasks &gt; Extract Data-tier Application  
A DAC pack is only the structure, not the data within it. It will however contain users and logins, etc. Helpful for when taking a system from DEV into Production or vice versa.  

Restore DAC Pack  
Server &gt; Right Click - Databases &gt; Deploy Data-tier Application  


In a dead lock, the dead lock victim will be the process with the least number of invested resources.  
Error code 1205 is dead lock victim


To handle dead locks, you should program defensively. An example would be a try catch.  
Example IF statement to put in catch of a Try - IF (ERROR_NUMBER() = 1205)  
http://technet.microsoft.com/en-us/library/ms179296(v=SQL.105).aspx


----------

###Mirroring, AlwaysOn and Replication Overview

####Mirroring
Mirroring is at database level. Make a change on Server A and the change is made straight away on server B, compared to log shipping which is on a schedule. You can only Mirror 1 to 1. Log shipping is 1 to many relationship for replicas.
You can automate failover between the two server, though you require a third server to act as a witness to manage, which is to prevent split brain situations.
Principal Server - Primary / Active
Mirror Server - The server is in a state of no recovery. You then use the mirror wizard to bring the database online with 'WITH RECOVERY'.


####Clustering
Clustering is server level. Uses a shared database between the two nodes on a shared storage medium. Active/Passive.


####AlwaysOn
Nodes use windows clustering services (install it) though you don't actually setup the cluster. The AlwaysOn configuration within SQL configures and manages the cluster. In AlwaysOn, you can have one principal server and up to 4 mirror/replica servers. You can set each replica to different modes - synchronous or asynchronous.
The LSN is maintained across all the nodes so you can use bits of logs from all of them in a DR scenario.
I believe that if a log is missing from a node or page, then the nodes will first ask the principal server for the missing log, else if not found, it will then broadcast to the whole group asking for log information.

Simply talk AlwaysOn configuration
https://www.simple-talk.com/sql/database-administration/sql-server-2012-alwayson/
Brent Ozar AlwaysOn Configuration
http://www.brentozar.com/sql/sql-server-alwayson-availability-groups/


####Replication
You only replicate data, doesn't include system data. The original source is called the publisher and the second node is called the subscriber. There is also another feature/service called Distributor which controls the flow. With the below three modes, the network can be flaky as it will simply catch up on its own.
There are three types of replication:
1. Snapshot - Can have pull or push replication.
2. Transactional - On the Distributor, theres an log read agent that reads items flagged in the publishers log files that its been told to track/read then replays on subscriber. This is what PL call Log shipping I believe?
3. Merge - Merges changes between both nodes. Uses GUI Global Unique Identifier to identify row changes, it will add a column for this to monitor for changes between the two servers.

There is a four though not widely really used mode that uses MSDTC. This will make the transaction on all or none at all. Financial systems might use this.

'Not For Replication' on a Trigger is handy as it means transactions that get synced across will not fire the trigger when inserted from a synced log entry.

Design replication into the database if you can at the beginning.


http://www.microsoft.com/en-us/sqlserver/learning-center/virtual-labs.aspx

mark.holmes@qa.com
