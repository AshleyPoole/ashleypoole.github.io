---
layout: post
title: JAMS Database Notes
date: 2014-04-06 20:39
categories: [devops, server-admin]
---
These are my notes regarding the installation of the JAMS database.

JAMS requires SQL Server and can be configured to use an existing SQL Server instance or install a SQL Express instance for you as part of the installer. If selecting an existing SQL Server instance and using Windows Authentication on your instance, then the user running the the installation must have access to create databases.
<h4>Data Files</h4>
When the database is created three data (MDF) files and file groups are created:
<ol>
	<li>Primary - Default.</li>
	<li>JAMS_History - Stores history information.</li>
	<li>JAMS_Volatile - Volatile and throw away information.</li>
</ol>
<h4>Security</h4>
As part of the installation, two database roles are created called JAMSApp and JAMSReader. The first has basically has SELECT, INSERT and UPDATE where as JAMSReader only has SELECT to certain tables.

A new user is created within the JAMS database called JAMSService which by default is mapped to administrators AD group.

You may want to review these permissions and users, and adjust to your needs. This may including creating a process user for the JAMS service and granting this DBO, then deleting the other roles.


If you need to re-install the DB for JAMS follow the below instructions:
<ul>
	<li>From the install directory of JAMS Scheudler, delete the common.settings file</li>
	<li>Run JAMSDBA as an administrator</li>
	<li>Type 'install' and enter</li>
	<li>Follow the wizard</li>
</ul>
