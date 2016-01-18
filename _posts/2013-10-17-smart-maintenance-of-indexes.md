---
layout: post
title: Smart maintenance of Indexes
date: 2013-10-17 17:30
categories: [databases]
tags: [sql-server, webinars]
---
Performance plans in MSSQL Server are a great way to keep databases in peak, tip top condition. MSSQL Server does a great job of rebuilding indexes (or reorganizing them) but let's face it, it's not that smart when it comes down to whether the index actually needs rebuilding or not!
This is where a stored procedure written by Ola Hallengren comes into play, that has won multiple awards. Ola has written a procedure called IndexOptimize which takes care of rebuilding and reorganizing indexes as well as updating statistics. The procedure is supported on SQL Server 2005 through till SQL Server 2012.

The procedure when run is passed two parameters as well as many others indicating two levels of fragmentation (@FragmentationLevel1 and @FragmentationLevel2). The procedure will then check the actually fragmentation percentage before deciding whether to carry out a reorganize or rebuild based on what you have set (@FragmentationLow, @FragmentationMedium and @FragmentationHigh).

Example execution:

{% highlight sql %}
EXECUTE dbo.IndexOptimize
@Databases = 'USER_DATABASES',
@FragmentationLow = NULL,
@FragmentationMedium = 'INDEX_REORGANIZE,INDEX_REBUILD_ONLINE,INDEX_REBUILD_OFFLINE',
@FragmentationHigh = 'INDEX_REBUILD_ONLINE,INDEX_REBUILD_OFFLINE',
@FragmentationLevel1 = 5,
@FragmentationLevel2 = 30
{% endhighlight %}

Below is example output from the script which is stored in a table called dbo.CommandLog that is creates.
** Image Not Added **

There are a lot more parameters you can play with which can been seen in the below link under resources for Ola's procedure.

<strong>Resources</strong>
MSSQL Server Reorganise and Rebuild of Indexes - <a href="http://technet.microsoft.com/en-us/library/ms189858.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/ms189858.aspx</a>
Ola Hallengren's IndexOptimize procedure - <a href="http://ola.hallengren.com/sql-server-index-and-statistics-maintenance.html" rel="nofollow">SQL Server Index and Statistics Maintenance</a>
