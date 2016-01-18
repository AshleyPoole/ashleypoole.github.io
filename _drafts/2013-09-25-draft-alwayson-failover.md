---
layout: post
title: AlwaysOn Failover
date: 2013-09-25 14:35
categories: [High Availability, Knowledge Base, Uncategorized]
---
As we would most likely want to use synchronous commit mode over asynchronous commit mode, I believe the best practice would be to take the databases offline on the primary node during a controlled failover. This would therefore prevent data loss during the migration.

Note – Asynchronous would require the hardening of the transactions on each node running as an  asynchronous member before returning success to the client executing the transaction. During network outages or disruptions this would therefore create more issues. Also from testing, failure to take the database offline on the primary node will result in transactions being killed.
<ol>
	<li>Take the databases offline on the current primary node. This can be achieved using the following command for all databases part of the AlwaysOn high availability group:
ALTER DATABASE &lt;databasename&gt;  SET OFFLINE WITH ROLLBACK IMMEDIATE</li>
	<li>Now wait a few minutes to ensure that transactions have been pulled a cross to the secondary nodes. You should also check the status from the AlwaysOn dashboard.</li>
	<li>On the secondary node you wish to become the primary run</li>
</ol>
ALTER AVAILABILITY GROUP [GROUPNAME] FAILOVER;

Powershell commands - <a href="http://technet.microsoft.com/en-us/library/ff878391.aspx">http://technet.microsoft.com/en-us/library/ff878391.aspx</a>
