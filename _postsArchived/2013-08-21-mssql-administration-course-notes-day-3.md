---
layout: post
title: MSSQL Administration Course Notes – Day 3
date: 2013-08-21 19:07
categories: [databases, knowledge-base]
tags: [training]
---
###Yesterdays Review

Show information about the oldest open transaction
DBCC OPENTRAN('tempdb')

Backup Options
WITH NOINIT - Append to files contents if already exists. One file, multiple backups contained.
WITH INIT - Overwrite file contents if already exists

A differential backup uses extents - 64K block sizes - 8 pages

A point in time restore only works with the 'WITH RECOVERY' option. This is due to it expects a point in time restore to be the last action carried out as you replay the logs. Once executed with 'WITH RECOVERY' the database will be brought online.

If you need to backup the tail log after datafile failure, you will need to use the 'CONTINUE_AFTER_ERROR' option with the log backup as normally it will fail due to the data file issues.

When starting a SQL Server instance in single user mode, also disabled SQL Server Agent as this will most likely log in first and take the single user connection.

----------

###AM Session - Authenticating, Authorising Users and Roles

There are roles on the server level as well as the database level. Roles on the database level are obviously unique to each database.

Authenication - Who are you  
Authorization - What can you access


Principals - Users and logins  
Securable - Objects - Like Schema, tables

SQL logins maps to database users. Users are a subset of the logins though both can exist without the other though will not function. There is an exception to this rule, see paragraph below on Database containment.

There is a new feature in SQL Server 2012 called Containment Level which is set on the database level. The default value for the containment level is 'none'. When the containment level is set to 'partial' (In 2012 this is currently the only other option), a number of changes occur to the database with one being that users specified on the database without a SQL login can access the database.

A side affect of using the partial database containment level is that when operating in this mode, connections made to a database in partial containment level may NOT access other databases, even if they are too in partial containment mode and the database user exists there too. Another disadvantage is that a using a partial containment level means the database cannot be used in replication.
Also when connecting to a database in partial containment level using SSMS, you must specify the database on log in.

To modify database containment level follow the below in SSMS:
Server Instance &gt; Databases &gt; 'DatabaseName' &gt; Properties (Right Click DB) &gt; Options &gt; Containment Type

Double Hop Problem - To fix the double hop problem you must configure SPN's so that the users token is passed along the chain when making the requests. This related to Kerberos. You use SETSPN.exe from the Windows Resource Kit.

<a href="http://technet.microsoft.com/en-us/library/ms191153.aspx" target="_blank">http://technet.microsoft.com/en-us/library/ms191153.aspx</a>

Only in SQL 2012, can you configure it to automatically register SPN's.
<a href="http://sqlmate.wordpress.com/2012/04/16/managed-service-accounts-with-microsoft-sql-server-2012/" target="_blank">http://sqlmate.wordpress.com/2012/04/16/managed-service-accounts-with-microsoft-sql-server-2012/
</a>

Impersonation - Act as another user on the local machine  
Delegation - Act as another user on remote machines

If a user to be use able to impersonate you must grant them the right first. Members of the sysadmin role will be able to impersonate all users without having to specify the right. <a href="http://sommarskog.se/grantperm.html" target="_blank">http://sommarskog.se/grantperm.html</a>

To execute SQL as another user, you need to run the SQL statement below. Once finished then run the REVERT statement.

{% highlight sql %}
EXECUTE AS USER = 'JohnSmith';

REVERT;
{% endhighlight %}

You can always check what user you are currently running as by running the first statement. Running the second status will show you the true underling login connected.

{% highlight sql %}
SELECT suser_user();

SELECT ORIGINAL_LOGIN();
{% endhighlight %}

When restoring databases or troubleshooting logging in issues, you may find that the SID's for the users do not match. You can check this with the query below. You will also need to fix this by ensuring the SID's on the login and user object match up. Note using sp_change_users_login is the old method which has now been superseded by ALTER USER.

{% highlight sql %}
SELECT sp.name, sp.principal_id, sp.sid, dp.principal_id, dp.sid
FROM sys.server_principals sp
INNER JOIN sys.database_principals dp
ON sp.name = dp.name
WHERE sp.name = 'TestUser';

ALTER USER [TestUser] WITH LOGIN = [TestUser];
{% endhighlight %}

<a href="http://technet.microsoft.com/en-us/library/ms174378.aspx" target="_blank">http://technet.microsoft.com/en-us/library/ms174378.aspx</a>

Login and User management snippets

{% highlight sql %}
-- Create a Windows SQL login
CREATE LOGIN 'DOMAINUSERNAME'
FROM WINDOWS WITH DEFAULT_DATABASE='MyProdDatabase';

-- Create a SQL login with the password check policy disable
CREATE LOGIN 'JohnSmith' WITH PASSWORD = 'SECRETPASSWORD', CHECK_POLICY = 'OFF';

-- Delete a SQL login
DROP LOGIN 'DOMAINJohnSmith';

-- Create a user on a database that already has a SQL login
CREATE USER 'JohnSmith' FOR 'JohnSmith';

{% endhighlight %}

CREATE LOGIN - <a href="http://technet.microsoft.com/en-us/library/ms189751.aspx" target="_blank">http://technet.microsoft.com/en-us/library/ms189751.aspx</a>

Role and Permissions management snippets

{% highlight sql %}
-- Grant access to an object.
GRANT SELECT,UPDATE,INSERT ON ['MyProdDatabase'].['ObjectNameLikeTable']
TO 'DomainJohnSmith';

-- Grant all access to an object.
GRANT All ON ['MyProdDatabase'].['ObjectNameLikeTable']
TO 'DomainJohnSmith';

-- Grant select, update, insert and reference access to an object.  
-- REFERENCE is needed if the table uses a PK/FK relationship. REFERENCE allows this relationship  
-- to work without the user needing access to PK/FK tables.  
GRANT SELECT,UPDATE,INSERT,REFERENCE ON ['MyProdDatabase'].['ObjectNameLikeTable']
TO 'DomainJohnSmith';

-- Grant a user a permission with the permission to grant others this permission. If the first  
-- users permission is revoked, then you can cascade this down.  
GRANT UPDATE ON ['MyProdDatbase'].['Customers']  
TO 'DomainJohnSmith'  
WITH GRANT OPTION;

-- Revoke UPDATE and INSERT access for an object from a user.  
REVOKE UPDATE,INSERT ON ['MyProdDatabase'].['ObjectNameLikeTable']  
FROM 'DomainJohnSmith';

-- Revoke UPDATE and INSERT access for an object from a user and cascade if the user was  
-- given 'WITH GRANT'.  
REVOKE UPDATE,INSERT ON ['MyProdDatabase'].['ObjectNameLikeTable']  
FROM 'DomainJohnSmith' CASCADE;

-- DENY access to an object. Helps if a member from a group must has less access  
-- and there are only a few items to manage, else best to create a new role.  
DENY UPDATE ON ['MyProdDatabase'].['ObjectNameLikeTable']
TO 'DomainJohnSmith';

-- Add a user to a role, replace ADD with DROP to remove. Pre SQL Server 2012 uses sp_addrolemember  
ALTER ROLE [db_owner] ADD MEMBER 'JohnSmith';

-- Grant permissions on column level - God help you though!!  
-- Note - a column GRANT will always work even with a DENY at the table level or above.  
GRANT SELECT ON 'MyTable' ('column1','column2') TO 'JohnSmith';  
{% endhighlight %}

GRANT - <a href="http://technet.microsoft.com/en-us/library/ms187965.aspx" target="_blank">http://technet.microsoft.com/en-us/library/ms187965.aspx</a>

In an example in the above code block, you can grant permissions on the column level. It may be best to create a view to hide columns rather than enter the world of column permissions as they may become hard to manage.

In SQL Server 2012 it is now possible to create your own roles.

Table function return a dataset so the user requires the SELECT permission.  
Scalar functions return a single value which requires the user to have the execute permission.

When trying to troubleshoot permission issues, you will need to look at the user token which can be used to track which role membership has given them access to the resource in question.

{% highlight sql %}
SELECT * FROM SYS.USER_TOCKEN;
{% endhighlight %}

A good link to some SQL for reporting on permissions <a href="http://consultingblogs.emc.com/jamiethomson/archive/2007/02/09/SQL-Server-2005_3A00_-View-all-permissions--_2800_2_2900_.aspx" target="_blank">http://consultingblogs.emc.com/jamiethomson/archive/2007/02/09/SQL-Server-2005_3A00_-View-all-permissions--_2800_2_2900_.aspx</a>

----------

###PM Session - Auditing</strong>

The old fashion of auditing was using triggers though there were limitations with using triggers. These limitations and side affects included no triggers available for SELECTS, performance impacts and the order in which triggers were fired couldn't be guaranteed. Also triggers couldn't be disabled, only deleted.

Example DML trigger

{% highlight sql %}
CREATE TRIGGER TR_Product_Quantity_Update
ON dbo.Product
FOR UPDATE
AS
BEGIN
   SET NOCOUNT ON;

   IF UPDATE(Quantity) BEGIN
      INSERT dbo.ProductQuantityAudit (ProductID, OldQuantity, NewQuantity)
      SELECT i.ProductID, d.Quantity, i.Quantity
      -- Inserted is new data
      FROM inserted AS i
      -- Deleted is the old data
      INNER JOIN deleted AS d
      ON i.Quantity = d.Quantity;
   END;
END;
{% endhighlight %}

Another method previously used though is being phrased off in future versions of SQL Server is SQL Server Profiler with is GUI tool. The syntax version of this is basically SQL Trace. SQL Server Profiler has been superseded by Extended Events.
Extended Events is the general event handling system used by SQL Server.
http://technet.microsoft.com/en-us/library/ms181091.aspx
http://technet.microsoft.com/en-us/library/bb630354(v=sql.105).aspx

SQL Server Security Auditing was first introduced in SQL Server 2008 and is not turned on by default. To create a new audit use the below instruction. Note, once created, you will need to enable the audit. You can also create the audit on the database level too underneath the database, then security.
SSMS &gt; Server &gt; Security &gt; Audits &gt; Right Click - New Audit

Change Data Capture (CDC) - Tracks changes to data and gives a before &amp; after view as the change happens. By default, CDC is not enabled and naturally is unable to track who actually changed the data. If tracking of who did what is needed, you will need to link CDC into something like Extended Events. CDC only tracks changes, so therefore doesn't track SELECT statements.
http://www.mssqltips.com/sqlservertip/1474/using-change-data-capture-cdc-in-sql-server-2008/

Change Tracking (CT) - Tracks changes to rows, though not the data that actually changed. CT is not enabled by default.
http://www.mssqltips.com/sqlservertip/1819/using-change-tracking-in-sql-server-2008/

----------

###Misc
CLR Assemblies written in the .NET framework can be loaded into SQL Server and can be accessed from functions, etc. This allows for some application logic to be moved into SQL Server and to also expand it's functionality.  
When creating the assembly in SQL Server, you need to set the Run Type which sets what resources the assembly can access.  
Safe - Has no access to external resources. SQL only.  
External Access - Can access external resources like file system and network.  
Unsafe - Same as external access though can also run unmanaged code. In SSMS, Unsafe is called unrestricted in the UI.  
<a href="http://technet.microsoft.com/en-us/library/ms345101.aspx" target="_blank">http://technet.microsoft.com/en-us/library/ms345101.aspx</a>

This can have positive performance effects as it doesn't have to calculate the row counts and also reduces messages sent to client every each query in a proc.

{% highlight sql %}
SET NOCOUNT ON;
{% endhighlight %}
