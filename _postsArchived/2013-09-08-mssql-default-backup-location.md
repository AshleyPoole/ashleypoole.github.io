---
layout: post
title: MSSQL Default Backup Location
date: 2013-09-08 09:24
categories: [databases, server-admin]
tags: [sql-server]
---
Default backup location

{% highlight sql %}
-- Query location
EXECUTE [master].dbo.xp_instance_regread N'HKEY_LOCAL_MACHINE', N'SOFTWAREMicrosoftMSSQLServerMSSQLServer', N'BackupDirectory'
-- Set location
EXECUTE [master].dbo.xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'SOFTWAREMicrosoftMSSQLServerMSSQLServer', N'BackupDirectory','REG_SZ', 'SERVERNAMESHARENAMEFOLDERNAME'
{% endhighlight %}


The same can be done using Regedit
Local Machine &gt; Software &gt; Microsoft &gt; Microsoft SQL Server &gt; 'INSTANCENAME' &gt; MSSQLServer
Key = BackupDirectory
