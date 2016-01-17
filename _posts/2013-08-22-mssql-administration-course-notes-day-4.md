---
layout: post
title: MSSQL Administration Course Notes – Day 4
date: 2013-08-22 19:53
categories: [databases, knowledge-base]
tags: [training]
---
###Yesterdays Review

In SQL 2012 you can now be a user on a database without a SQL login. This requires containment level to be set to partial at the database level.

Everyone is part of the Public role (server or database level).

Sysadmin at the server level maps to dbo in databases. Sysadmins have the abilities to impersonate all users be default.

If running proc which executes as a user, the user running the proc must have impersonate access to the user the proc is running under.

If you write audit events to a file, you can only truly read it from within SQL using the function sys.fn_get_audit_file.

CDC - This records data changes and what the changes were.

----------
###AM Session - Automating Server Management With SQL Server Agent

Setup Database Mail  
Server &gt; Management &gt; Database Mail &gt; Right Click - Configure Database Mail
If you grant a user the 'DatabaseMailUserRole' on a database level you can create them a private database mail profile.

Configure SQL Agent To Use Database Mail  
Server &gt; Right Click - SQL Server Agent &gt; Properties &gt; Alert System
Also configure failsafe operator. This is useful if used pager email operators and no one is on duty. Use pager email addresses so you can schedule peoples hours.

Keep on top of house keeping for mail log size

<a href="{{ "/img/2013/08/Capture58-Mini-CleanUpLogs.png" | prepend: site.assetsbaseurl }}"><img src="{{ "/img/2013/08/Capture58-Mini-CleanUpLogs.png" | prepend: site.assetsbaseurl }}" alt="Capture58-Mini-CleanUpLogs" width="294" height="144" class="alignnone size-full wp-image-102" /></a>

SQL Agent operators are humans / email addresses that will receive emails.


Alerts in a SQL Agent Job are like a trigger / monitor, not an notification.

Three type of alerts:
Standard Error Messages
Stat Counters
WMI Event

Standard Error Codes - 1033 is english and severity 19 or above is classed as fatal errors.
{% highlight sql %}
SELECT * FROM sys.messages
WHERE language_id=1033
{% endhighlight %}

message ID 50000 &gt; Is a custom message
Can add a custom message
{% highlight sql %}
sp_add_message
{% endhighlight %}


For a TSQL type Agent Job, you need to set run as under advance menu/page, not general. Below will walk through creating a new credential.

Map an credential to an underlying account. You map a Windows account to a credential within SQL Server. Note passwords are manually managed / stored within SQL Server.
Server &gt; Credentials

You then have to bind the credential to what it will be used for and is given a name a proxy
Server &gt; SQL Agent &gt; Proxies &gt; New


SSMS can only connect to one instance of SSIS on a machine so you have to mess around with the below ini then restart SSIS Service for SSMS to connect. In the XML you're looking for &lt;ServerName&gt;&lt;/ServerName&gt;
C:Program FilesMicrosoft SQL Server110DTSBinnMsDtsSrvr.ini.xml


House keeping for agent error logs - This happens on restart of the service or you can run the below command to manually cycle the logs.
{% highlight sql %}
exec sys.sp_cycle_errorlog
{% endhighlight %}


For an alert to send out an email if configured, then it must enabled in sys.messages by settings is_event_logged column to 1:
{% highlight sql %}
SELECT * FROM sys.messages

-- Returns list of events being logged
SELECT * FROM sys.messages
WHERE is_event_logged &lt;&gt; 0
{% endhighlight %}


----------

###PM Session - Ongoing DB maintenance

DBCC CHECKDB  
Checks logical and physical integrity in the database
<a href="{{ "/img/2013/08/Capture64-ddbc_checkdb_options.png" | prepend: site.assetsbaseurl }}"><img src="{{ "/img/2013/08/Capture64-ddbc_checkdb_options.png" | prepend: site.assetsbaseurl }}" alt="Capture64-ddbc_checkdb_options" width="365" height="264" class="alignnone size-full wp-image-105" /></a>


{% highlight sql %}
-- DBCC CHECKDB  For Errors Only
DBCC CHECKDB('mydatabase') WITH NO_INFOMSGS

-- Repair without dataloss -
-- Note: Single user mode is required
DBCC CHECKDB('mydatabase', 'REPAIR_REBUILD');

-- Repair with DATALOSS!! This just deletes the issue.
-- Note: Need to be in single user mode on the database for this to prevent issues when running
DBCC CHECKDB('mydatabase','REPAIR_ALLOW_DATA_LOSS');
{% endhighlight %}

A DBCC CHECK with TABLOCK will run much quicker as it isn't using database snapshots though will lock the table while running. This therefore may cause user impact for large tables if running during office hours.



824 error indicates a detected logicalic consistency IO based issue. Always check reading certain records if done a SELECT * to get a 824. You can see page errors in the below table. In the enterprise edition of SQL Server, you are able to do page restores.
{% highlight sql %}
SELECT * FROM MSDB.dbo.suspect_pages
{% endhighlight %}


Tip - Defragment within SQL not on the OS level as it's not SQL aware.

----------
###PM Session - Indexes

Clustered Index - Only one per table - Creating a PK by default creates a clustered index in the background.
Note - If a clustered index is created on a heap with several existing non-clustered indexes, all the non-clustered indexes must be rebuilt so that they contain the clustering key value instead of the row identifier (RID). Similarly, if a clustered index is dropped on a table that has several non-clustered indexes, the non-clustered indexes are all rebuilt as part of the DROP operation. This may take significant time on large tables.
http://technet.microsoft.com/en-us/library/ms186342.aspx

Non-clustered Index - You can have up to 999 indexes. Non-clustered indexes contain the clustering key value instead of the row identifier.

non-left level - index pages
left level - is the data
http://blogs.developpeur.org/raptorxp/pages/sql-server-data-structures.aspx


// AVERAGE_FRAGMENTATION_IN_PERCENT //  

<strong>REBUILD INDEX</strong>  
MS say less than 30% use REORGANIZE above use REBUILD though might be better to wait till 50% for REBUILD. Each system and requirement are unique.

Physical - Locks for the duration. Transaction log intensive
{% highlight sql %}
ALTER INDEX CL_LogTime ON dbo.LogTime REBUILD;
{% endhighlight %}

Use fillfactor on the index rebuild to leave free space on each page so that if new items are added it gives the option for the index to have space to insert in the correct place to short term help with preventing fragmentation. This example will leave 80% free on each page.
{% highlight sql %}
ALTER INDEX CL_LogTime ON dbo.LogTime REBUILD WITH (FILLFACTOR = 80);
{% endhighlight %}

Logical - Can access data while running. Will skip page if locked, by user for example. Can get index_name from sp_helpindex [TABLENAME]
{% highlight sql %}
ALTER INDEX [INDEX_NAME] ON dbo.LogTime REORGANIZE;
{% endhighlight %}


----------

###PM Session - Maintenance Plans

Server &gt; Management &gt; Maintenance Plans
Pretty straight forward. Not really much notes needed as maintenance plans reference other system functionality.


<strong>Misc Links</strong>  

Good script as health check for when taking over a new server
http://www.brentozar.com/blitz/

http://www.codeplex.com
http://bidshelper.codeplex.com/
http://www.idera.com/productssolutions/freetools/powershellplus
http://ola.hallengren.com/
