---
layout: post
title: MSSQL AlwaysOn Notes
date: 2013-08-26 14:30
categories: [databases]
tags: [sql-server, notes]
---
#### Database Eligible  
To be eligible to be included in an Availability Group, databases must meet the following prerequisites:  
· Be a user database.  
· Be a read/write database.  
· Be a multi-user database.  
· Not use AUTO_CLOSE.  
· Use the full recovery mode.  
· Possess a full database backup.  
· Reside on the SQL Server instance where you are creating the availability group and be accessible.  
· Not belong to another availability group.  
· Not be configured for database mirroring.  


####Commit Modes
<strong>Synchronous Commit</strong>  
The primary waits for the secondary replica to harden the transaction in it's log file, before the primary will send a transaction confirmation to the client. This method emphasises HA over performance though does allow for automatic failover as commits are confirmed on the secondary which works to prevent data loss / consistency.

<a href="{{ "/img/2013/08/Calwaysonsync.gif" | prepend: site.assetsbaseurl }}"><img src="{{ "/img/2013/08/alwaysonsync.gif" | prepend: site.assetsbaseurl }}" alt="Capture64-ddbc_checkdb_options" width="592" height="252" class="alignnone size-full" /></a>


<strong>ASynchronous Commit</strong>
The primary doesn't wait for any of the secondary replicas to harden the log. Once written to the local log the client sends the transaction confirmation to the client. Automatic failover is not possible with asynchronous commit mode due to the possibility of data loss - this is due to the commits are not confirmed on the secondaries.

----------

<strong>Automatic Page Repair</strong>
Each availability replica tries to automatically recover from corrupted pages on a local database by resolving certain types of errors that prevent reading a data page. If a secondary replica cannot read a page, the replica requests a fresh copy of the page from the primary replica. If the primary replica cannot read a page, the replica broadcasts a request for a fresh copy to all the secondary replicas and gets the page from the first to respond. If this request succeeds, the unreadable page is replaced by the copy, which usually resolves the error.
http://technet.microsoft.com/en-us/library/cf2e3650-5fac-4f34-b50e-d17765578a8e


<strong>Monitoring</strong>
sys.dm_hadr_availability_group_states - Returns a row for each AlwaysOn group that the local instance is part of and the remote status of the groups.
sys.dm_hadr_availability_replica_states - Returns a row for the all replicas in the same AlwaysOn group with their status.
sys.dm_hadr_database_replica_states - Returns a row for each database that is participating in a AlwaysOn group on the local instance with their status.



<strong>Labs</strong>
http://www.microsoft.com/en-us/sqlserver/learning-center/virtual-labs.aspx


<strong>Misc Links</strong>
http://technet.microsoft.com/en-us/library/hh510230.aspx
http://technet.microsoft.com/en-us/library/ff877931.aspx
