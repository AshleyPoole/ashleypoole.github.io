---
layout: post
title: SQL Server 2014 In-Memory OLTP "Hekaton"
date: 2013-11-16 16:45
categories: [databases, knowledge-base]
tags: [sql-server]
---
Well first off, I would like to say a big thanks to all the organisers and sponsors of SQLRelay 2013 that made the event possible! For those who don't know, SQLRelay is a free annual SQL Server 1 day conferences that is run over two weeks throughout the UK by the community for the community. More information can be found at <a title="sqlrelay" href="http://www.sqlrelay.co.uk" target="_blank">http://www.sqlrelay.co.uk</a>.

This year (the first year I attended) was packed with great sessions including using Powershell with SQL Server and the use of data partitioning with creative uses. Another great session was on SQL Server 2014 In-memory OLTP (code name Hekaton) by Scott Klein who's an technical evangelist for Microsoft.

In-memory OLTP is an memory optimised database engine which can achieve significant performance increase over disk based tables. When tables or stored procedures are converted to In-memory based objects they are compiled into native code in .DLL format, then loaded into memory. When starting SQL Server 2014, you may have to a wait a short period of time while those objects are loaded into memory so they could be available straight away depending on the size of the object.

In-memory objects will appear within management tools such as SQL Server Management Studio as regular objects just like an other table or stored procedures would. One big disadvantage however is that stored procedures which are In-memory can only access in memory tables. This will therefore will be an important consideration when deciding what stored procedures to place In-memory (tables are unaffected by this, though In-memory tables that reference disk based tables will slowed by this disk based IO) .

Microsofts best practice is to allow 2 times the tables size for memory allocation when converted to an In-memory table, though this could be greatly different depending on your environment and tables growth rate. Currently SQL Server 2014 CTP2 will NOT check available memory when converting objects so you must ensure you have the correct amount of memory (RAM).

Out of the box, SQL Server 2014 will provide a database level report within SSMS (SQL Server Management Studio) that will suggest tables that would benefit from being In-memory. My current believe from the demo at SQLRelay 2013 is that the report will choose tables based on those accessed the most frequently, it will not however take into account tables which reference to each other using relationships or those most frequently accessed together in join statements for example.


CTP2 release limitations also include but not limited to:
<ul>
	<li><span style="line-height: 15.989583969116211px;">No DML triggers support for In-memory tables</span></li>
	<li>No ability to add or remove indexes without a drop/recreate of In-memory table</li>
	<li>Maximum of 8 indexes on a In-memory table</li>
	<li>Statistics for In-memory tables aren't automatically updated</li>
</ul>
You also cannot have a data partition in-memory with another on disk though I believe this would be a superb feature if Microsoft did decided to implemented support for it. Such a feature would allow for historical data to be on slow disk based storage while more recent data stored within memory while being managed using partitions. Currently the only way to achieve something similar with SQL Server 2014 would be to move historical data to another table so that the table holding the most recent data could then be moved to In-memory.

All this talk of Hekaton (In-Memory) tables does make me wonder about if simply using a RAM Disk solution would be easier, especially if we are talking about tempdb for example where we don't care if it gets thrown away after a reboot. For databases / tables where integrity matters it would admittedly get messy trying to use a RAM Disk though I believe it could be done with a bit of magic - highly unrecommended especially in productions environments.

Note - This article is meant to be a high level overview. For more information of usage, I recommend the below resources. The second resource from thinknook.com should get you up and running within the shortest amount of time.

<a href="http://blogs.technet.com/b/dataplatforminsider/archive/2013/06/26/getting-started-with-sql-server-2014-in-memory-oltp.aspx" target="_blank">http://blogs.technet.com/b/dataplatforminsider/archive/2013/06/26/getting-started-with-sql-server-2014-in-memory-oltp.aspx</a>
<a href="http://thinknook.com/in-memory-memory-optimized-tables-and-oltp-with-sql-server-2014-2013-07-12/" target="_blank">http://thinknook.com/in-memory-memory-optimized-tables-and-oltp-with-sql-server-2014-2013-07-12/</a>
