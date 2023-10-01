---
layout: post
title: MSSQL Administration Course Notes - Day 1
date: 2013-08-19 18:10
categories: [databases, devops]
tags: [sql-server, notes]
---
<strong>Check collation</strong>

{% highlight sql %}
SELECT SERVERPROPERTY('collation')
SELECT DATABASEPROPERTYEX('tempdb','collation')
{% endhighlight %}


OR
SSMS &gt; Server &gt; Properties &gt; Server Collation
CI means case insensitive

<strong>Quick way to format an unformatted query</strong>
http://www.dpriver.com/pp/sqlformat.htm

<strong>Restart SQL Services</strong>
Use SQL Server Configuration Manager over Admin Tools &gt; Services for making changes to SQL instances. In rare examples, it doesn't work correctly like updating service/process user if using Administration Tools &gt; Services (Services.msc).

<strong>Forcing port number on named instance</strong>
SSCM &gt; Network Configuration &gt; INSTANCE &gt; IP ADDRESSES &gt; IPALL &gt; Change port number

<strong>SQL Aliases</strong>
SSCM &gt; Native Client Configuration &gt; Aliases
Note - SSMS is a 32Bit application so need to create alias in SSCM &gt; Native Client Configuration (32bit) &gt; Aliases. This needs the SQL Server Browser to be started as this is what handles the direction.
http://blogs.msdn.com/b/sqlro/archive/2009/06/16/how-to-deploy-sql-server-client-aliases-using-active-directory-gpo-adm.aspx

<!--more-->
<strong>SQL Server Architecture - Sizing requirements</strong>
Guess work really! Learn from experience as guidelines vary a lot!

<strong>SQLIO Testing IO Tools - unsupported though supplied by MS</strong>
SQLIOSIM - GUI
SQLIO - command line
http://www.brentozar.com/archive/2008/11/storage-performance-testing-with-sqlio/

<strong>Install Logs</strong>
%programfiles%microsoft sql server110setup bootstraplog

<strong>Move to new server</strong>
SSIS could be used instead of backup and restore to new server. SQL 2005 or above.

<strong>Store/show files in the db though also on the file system - Need to look more at this</strong>
Filestream + filetable

<strong>Alter log size</strong>

{% highlight sql %}
ALTER DATABASE tempdb
MODIFY FILE ( NAME = templog, SIZE = 10MB);
GO
{% endhighlight %}

&nbsp;

<strong>Move system databases</strong>
http://support.microsoft.com/kb/224071
