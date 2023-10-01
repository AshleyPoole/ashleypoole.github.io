---
layout: post
title: MSSQL Database Recovery
date: 2013-12-10 18:46
categories: [databases]
tags: [sql-server]
---
If you're unluckily enough to have a large transaction rolling back due to the SQL Server service crashed or someone restarted it, the below query will show you the estimated time remaining in Recovery for the database. The time reported can be volatile.

{% highlight sql %}
-- SQL Server 2008
DECLARE @DBName VARCHAR(64) = 'AdventureWorks2012'

DECLARE @ErrorLog AS TABLE([LogDate] CHAR(24), [ProcessInfo] VARCHAR(64), [TEXT] VARCHAR(MAX))

INSERT INTO @ErrorLog
EXEC sys.xp_readerrorlog 0, 1, 'Recovery of database', @DBName

SELECT TOP 5
	 [LogDate]
	,SUBSTRING([TEXT], CHARINDEX(') is ', [TEXT]) + 4,CHARINDEX(' complete (', [TEXT]) - CHARINDEX(') is ', [TEXT]) - 4) AS PercentComplete
	,CAST(SUBSTRING([TEXT], CHARINDEX('approximately', [TEXT]) + 13,CHARINDEX(' seconds remain', [TEXT]) - CHARINDEX('approximately', [TEXT]) - 13) AS FLOAT)/60.0 AS MinutesRemaining
	,CAST(SUBSTRING([TEXT], CHARINDEX('approximately', [TEXT]) + 13,CHARINDEX(' seconds remain', [TEXT]) - CHARINDEX('approximately', [TEXT]) - 13) AS FLOAT)/60.0/60.0 AS HoursRemaining
	,[TEXT]
FROM @ErrorLog ORDER BY [LogDate] DESC

-- SQL Server 2012
DECLARE @DBName VARCHAR(64) = 'AdventureWorks2012'

DECLARE @ErrorLog AS TABLE([LogDate] CHAR(24), [ProcessInfo] VARCHAR(64), [TEXT] VARCHAR(MAX))

INSERT INTO @ErrorLog
EXEC master..sp_readerrorlog 0, 1, 'Recovery of database', @DBName

SELECT TOP 5
	 [LogDate]
	,SUBSTRING([TEXT], CHARINDEX(') is ', [TEXT]) + 4,CHARINDEX(' complete (', [TEXT]) - CHARINDEX(') is ', [TEXT]) - 4) AS PercentComplete
	,CAST(SUBSTRING([TEXT], CHARINDEX('approximately', [TEXT]) + 13,CHARINDEX(' seconds remain', [TEXT]) - CHARINDEX('approximately', [TEXT]) - 13) AS FLOAT)/60.0 AS MinutesRemaining
	,CAST(SUBSTRING([TEXT], CHARINDEX('approximately', [TEXT]) + 13,CHARINDEX(' seconds remain', [TEXT]) - CHARINDEX('approximately', [TEXT]) - 13) AS FLOAT)/60.0/60.0 AS HoursRemaining
	,[TEXT]
FROM @ErrorLog ORDER BY [LogDate] DESC
{% endhighlight %}

Source : <a href="http://timlaqua.com/2009/09/determining-how-long-a-database-will-be-in-recovery-sql-server-2008" target="_blank">http://timlaqua.com/2009/09/determining-how-long-a-database-will-be-in-recovery-sql-server-2008</a>
More info : <a href="http://blogs.msdn.com/b/psssql/archive/2010/12/29/tracking-database-recovery-progress-using-information-from-dmv.aspx" target="_blank">http://blogs.msdn.com/b/psssql/archive/2010/12/29/tracking-database-recovery-progress-using-information-from-dmv.aspx</a>
