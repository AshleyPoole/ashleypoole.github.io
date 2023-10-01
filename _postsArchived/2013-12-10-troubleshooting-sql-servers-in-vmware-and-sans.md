---
layout: post
title: Troubleshooting SQL Servers in VMware and SANs
date: 2013-12-10 19:52
categories: [databases]
tags: [sql-server]
---
Name : Troubleshooting SQL Servers in VMware and SANs
Author : Brent Ozar
Webinar Date : 10-12-2013

<strong>How SQL Server Schedules CPU</strong>
A query will keep running until complete and will only let other queries run unless it has to wait for a resources (i.e. Disk), then SQL Server moves it into the waiting queue. There are three 'queues' - Running, Runnable, Waiting (Queued).

The newer way to track bottlenecks is to use waitstats, compared to the older way which was to look at CPU, compare metrics to MS best practice.
http://www.brentozar.com/sql/wait-stats/

Brent also recommends his AskBrent sp.

<strong>Storage</strong>
When we're waiting to read and write to storage, these will show high PAGEIOLATCH or ASYNC_IO_COMPLETION or WRITELOG (Log File). The fix to this doesn't necessary mean we need faster storage, allocating more RAM could help the issue.

These performance monitors that are important for storage from the server. If these show issues but aren't visible on the SAN it could be network.
Physical Disk : Avg Sec/Read
Physical Disk : Avg Sec/Write

sys.dm_io_virtual_file_stats will get the database / file level status. Also can use sp_AskBrent with expert mode turned on (sp_AskBrent @ExpertMode = 1).

<strong>Fixing PAGEIOLATCH waits</strong>
Fix these items in this order
1. tune indexes and queries
2. Add more RAM - Cheaper than adding CPU
3. Make storage faster (Haha! $$)

<strong>Fixing WRITELOG waits</strong>
1. Write less - Do we have scratch databases, tables, staging that could move to other databases that don't need to write to the log
2. Make storage faster - Or put logs on different volume

PAGELATCH is a light wait lock on a page. Before SQL Server can read a page it much check if anyone has a lock on that page. For high numbers of these we need to find the queries that are locking the pages and putting the creating the correct indexes.



<strong>Network</strong>
ASYNC_NETWORK_IO waits is not SQL Server. It's either network or the applications / servers that are accessing the data. You will need to identify the queries so you can back track the application or server.

<strong>Fixing SOS_SCHEDULER_YEILD in VMWare</strong>
SOS_SCHEDULER_YEILD or queries that are runnable though not running, will show that more cores are needed. Check Processor/% Used performance monitor counter too.
1. Update VMWare - VMware 4.1 has improvements for better CPU scheduling. Every release of VMware gets better!
2. Increase cost of parallelism
3. Minimise the number of vCPU's in each guest

BrentOzar.com/go/virtual
BrentOzar.com/go/san

<strong>QA</strong>
Fragmentation is rarely the issue

Servers running SQL Server should never touch the page file as this shows it needs more RAM. Some people have been reducing this to 2GB as this is the amount needed for Windows to create a mini dumb file.



"RAM covers so many sins"
