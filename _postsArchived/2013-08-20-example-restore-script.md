---
layout: post
title: MSSQL Example Restore Script
date: 2013-08-20 19:09
categories: [databases, knowledge-base]
tags: [sql-server]
---
This example is of what can be done with a automated restore script. This script example checks that the connected instance name is either MYDEV or MYDEV2.

{% highlight sql %}
USE master;
GO

-- Checking to ensure the instance being restored is in desired list
IF NOT SERVERPROPERTY('InstanceName') IN ('MYDEV', 'MYDEV2')
PRINT '*** ALLOWED SERVER INSTANCE NOT DETECTED -- ABORTING ***'
RETURN

-- Setting databases to single user mode and rolling back open transactions
ALTER DATABASE MyBigDatabase SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
GO

-- May need to kill current connections here though

-- Restoring with replace
RESTORE DATABASE MyBigDatabase
FROM DISK = 'C:TempSQLBackupsMyBigDatabase.bak' WITH REPLACE;

USE MyBigDatabase;
GO

-- Update any app settings here
UPDATE Application.Settings
SET enabled = 1
WHERE settingName = 'ApplicationRunning';


-- Remap Login and User SIDs - sp function has now been superseded by ALTER USER
-- sp_change_users_login 'update_one', 'MYUSER', 'MYUSER'
ALTER USER 'MYUSER' WITH LOGIN = 'MYUSER'


ALTER DATABASE MyBigDatabase SET MULTI_USER
GO
{% endhighlight %}
