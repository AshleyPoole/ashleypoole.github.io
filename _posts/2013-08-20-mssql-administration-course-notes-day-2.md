---
layout: post
title: MSSQL Administration Course Notes – Day 2
date: 2013-08-20 16:24
categories: [databases, knowledge-base]
tags: [training]
---
### AM Session - Backing up
<strong>Backup Types</strong>

Full - Everything.
Differential - Everything from last full backup
Copy - Just copies the files, no truncating of logs, etc
Filegroup - Can backup filegroups
File - Can select an certain data file to backup

Recovery Time Objective (RTO) - How long to recover
Recovery Point Objective (RPO) - What point can we recover from

<strong>Write Ahead Logging</strong>
Writes from memory to the log, then memory to the DB

<strong>CHECKPOINT</strong>
Confirms what has been written to the log has been written to the DB and if not, then updates DB to the transaction log.
This is a soft recovery point
The server will equalise on startup (Runs checkpoint)
SQL dynamically decides when to run this though a developer can run CHECKPOINT at anytime

<strong>Log Backups</strong>
When doing a successful log backups, it only empties the log, it won't actually shrink the data file. This must be done with a DBCC SHRINK command.
Each log entry is assigned a LSN - Log Sequence Number

<strong>Recovery Models</strong>
Simple - Used to be called truncate log on checkpoint. The master databases uses this mode and is hard coded thus can't be changed - This is done so that master cant fill up the log causing MSSQL to die.
Full - Requires log backups, Avoids data loss and allows restore to a point in time also.
Bulk Logged - Minimize the log entries for bulk operations. A normal change / update is logged as if it was set to full mode. This can be left selected all the time if don't need to use point in time restore. Some people switch to bulk logged during imports then switch back to full after a full backup (The backup is for consistency only, not a requirement. This is due to the transaction logs for the time during bulk logged mode won't be very detailed).

Creating a Primary key creates a clustered index

<strong>Partitioned Tables</strong>
http://technet.microsoft.com/en-US/library/ms345146(v=SQL.90).aspx

<strong>Set backups for a database to always be compressed as default</strong>
This increases CPU usage during the backup and restore process though reduces file size and disk IO

{% highlight sql %}
EXEC sp_configure 'backup compress default', 1;
GO
RECONFIGURE;
GO
{% endhighlight %}

Where are backup logs stored?
MSDB Database

See backup header - I.e what the BAK contains

{% highlight sql %}
RESTORE HEADERONLY
FROM DISK = 'C:temptest.bak';
GO
{% endhighlight %}

See what files were backed up in the BAK file

{% highlight sql %}
RESTORE FILELISTONLY
FROM DISK = 'C:temptest.bak';
GO
{% endhighlight %}
{% highlight sql %}
Shutdown INSTANCE
SHUTDOWN;
GO
{% endhighlight %}

Backup logs (tail-log) when data file is not available as corrupt for example. You need to use CONTINUE_AFTER_ERROR as it is seeing that the data file doesn't exist.

{% highlight sql %}
BACKUP LOG testdb
TO DISK = 'C:temptestdb.trn'
WITH INIT, CONTINUE_AFTER_ERROR ;
GO
{% endhighlight %}

----------
### PM Session - Restoring
<strong>Restore Steps</strong>

Backup Tail Log
Restore Backup
substep 1 - Data copy - Creates files and copies data into files
substep 2 - Redo - Applies committed transactions from restored log entries
substep 3 - Undo - Roll back uncommitted transactions
(Redo and Undo are called Recovery)
A database move be recovered before it can be brought online
Use 'WITH NORECOVERY' when restoring then which allows you to relay log files, manually bring online using with recovery when all done.

{% highlight sql %}
-- BACKUP TAIL LOG FIRST
RESTORE DATABASE testdb
FROM DISK = 'C:temptestdb.bak'
WITH NORECOVERY
RESTORE LOG testdblog
FROM DISK = 'C:temptestdb.trn'
WITH NORECOVERY
-- Once happy all transaction logs have been replayed
-- and want to bring the database online
RESTORE DATABASE testdb
WITH RECOVERY
{% endhighlight %}

Log Shipping replaced with Mirroring, Mirroring replaced with AlwaysOn.

Replication is objects within databases like just tables.

Log shipping only replays fully committed transactions just like checkpoint. During old style log shipping, open transactions are written to a undo file as they as cannot be replayed.

<strong>Restore to a transaction point</strong>
To use this option, the transaction must have first been marked when executed.

{% highlight sql %}
BEING TRAN WITH MARK
// Do some work here
COMMIT TRAN
{% endhighlight %}

<strong>Restore database without recovery</strong>
Restore trans log - here you can see marked transactions

{% highlight sql %}
RESTORE LOG testdb
FROM DISK
WITH RECOVERY, STOPBEFOREMARK = ''
// Or STOPATMARK -  WITH RECOVERY, STOPATMARK = ''
{% endhighlight %}

<strong>SQL Server Single User Mode</strong>
This can be configured using SSCM, add startup flag -m
This is helpful if the master db screws up and need to get into the instance to restore master. The instance will then shutdown, which you can then take it out of single user mode.

<strong>Restore database to another</strong>
Check that it's not going to restore over the files over the source databases - It should prevent this as the files should be locked.

<strong>Restore pages</strong>
Enterprise only - More to come latter in week on this topic

{% highlight sql %}
SELECT * FROM msdb.dbo.suspect_page
{% endhighlight %}


http://technet.microsoft.com/en-us/library/ms175168.aspx

----------
### Rebuilding system databases
First type to restore them from backups, else try the below.

This will give you a blank master, msdb and model database. You should then be able to restore your backups of those databases over the top.
Running the SQL installer again, will use copies of the blank databases from "C:Program FilesMicrosoft SQL ServerMSSQL10.MSSQLSERVERMSSQLBinnTemplates" which are placed there during the initial install. You CANNOT just copy the blank databases to replace the previous system databases on the NTFS level as the system databases are unique to each server, due to a certain type of unique ID code/coding. The SQL installer (setup.exe) adapts/recodes these blank system database copies for use on your instance. The below post contains the exact switches to run with the installer.
http://blogs.msdn.com/b/psssql/archive/2008/08/29/how-to-rebuild-system-databases-in-sql-server-2008.aspx

<strong>Every time you make changes to system databases MS recommend you should back them up!</strong>

A dirty and unsupported technique although DR companies use it, is to stop all SQL services and copy all system databases while they are working (i.e day 1 of the install) and use this as as backup so you can manually replace the broken data files for the system databases in DR scenario, as these are already aligned/coded to the server instance. Then update it by restoring the latest system database backups.

The resources databases can only be recreated using the setup. The resources database is not visible within SSMS as this is actually the core database for SQL server, which sits below master, msdb, etc.

This link also contains good items to look for if an SQL instance won't start
https://www.simple-talk.com/sql/backup-and-recovery/the-sql-server-instance-that-will-not-start/

----------
### Importing and exporting data
Extract Transform Load (ETL)

Bulk Copy Program (BCP) - Takes text files and dumps data
Bulk Insert - Syntax version of BCP
OPENROWSET(BULK) - relates to excel (I think)
Import/Export Wizard - Cut down version of SSIS. Copies data from source to source, like SQL to SQL.
XML Bulk Load - https://www.simple-talk.com/sql/t-sql-programming/xml-jumpstart-workbench/

Disable Index - Useful for when doing bulk uploads
ALTER INDEX MY_INDEX_NAME on MY_TABLE_NAME DISABLE
Enable Index
ALTER INDEX MY_INDEX_NAME on MY_TABLE_NAME ENABLE

Disable constraint- Useful for when doing bulk uploads
ALTER TALBE NOCEHECK CONSTRAINT
Enable constraint
ALTER TALBE CHECK CONSTRAINT
Enable constraint and check data - Will error if issue
ALTER TALBE WITH CHECK CHECK CONSTRAINT

BCP
bcp IN -T -f -S

<strong>Bulk Insert</strong>

{% highlight sql %}
BULK INSERT
FROM
WITH ( FORMATFILE=, BATCHSIZE=200, FIRSTROW=2 )
{% endhighlight %}

SQL Business Intelligence Development Studio - Now called Data management tools in 2012 which is ased on VS2010.

<strong>Work with a SSIS package save using SSMS when in database engine import/export tool</strong>
Create blank project for Integration Services then add existing package, if saved package from export using db engine. Saw basic examples of creating SSIS packages. Connect to Integration Services instead of Database Engine within SSMS to see packages, as when stored in SQL, they are placed in the MSDB database and have to been seen from Integration Services.

Data management tools uses double quotes (") as delimiter where as MSSQL uses single quote (')
Data management tools can be used for a wide range of things including transforms, etc.

----------
### Cleaning data without DQS / Highlight issues with the data that stand out

Using Data Management tools
New SSIS Project
Drag / use Data Profiling Task
Double click. Set destination, quick profile, view results
