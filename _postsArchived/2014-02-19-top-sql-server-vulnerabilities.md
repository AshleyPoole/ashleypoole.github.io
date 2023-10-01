---
layout: post
title: Top SQL Server Vulnerabilities
date: 2014-02-19 20:50
categories: [databases, knowledge-base]
tags: [webinar, sql-server]
---
Name : Top SQL Server Vulnerabilities
Author : Brian Kelley
Webinar Date : 19-02-2014

<h3>Improper or Default Configurations</h3>
sp_Biltz will show security issues as well as performance items

Centre for Internet Security - Security benchmarks
https://benchmarks.cisecurity.org/downloads/multiform/
<h3>Excessive Privileges</h3>
Principle of Least Privilege
<strong>Permission to do the job, but nothing more. Though, nothing less, as this threatens availability.</strong>

The Service Account (SA) shouldn't be a domain administrator, nor a local administration on the server. SQL Server doesn't need a local administrator account. Run SQL Server as a regular AD account. The installer will set the permissions needed for the user.

Don't reuse credentials across servers.

Review both sysadmin and securityadmin role as they can grant others access. Also audit CONTROL (CL)  permission. (sys.server_permissions WHERE type = "CL")

Who knows the SA password?

When restoring a database, check within the database to see who it thinks the owner this. This is sometimes different than what the server believes it is.

SQL Server Agent user does require SA.
<h3>Improper Security</h3>
Two types of securable's:
- Scopes
- Securable's themselves - objects / items

Apply permissions using database roles
<h3>Sensitive Data</h3>
Every database has a system table called sys.columns which lists columns for all tables in that database. You can use this to find sensitive columns easier without having to check each table. This will take a lot of work!

There are many options to deal with sensitive data though was out of scope for the webinar.
<h3>Outside of SQL Server</h3>
A local administrators can bring SQL Server up in a safe mode allowing them sysadmin access.

Who has access to the storage and backups?

Are backups encrypted? SQL Server Enterprise has a transparent encryption feature which protects the database files as well as the backups.

SQL Injection - Due to poor or non-existent input validation and usually exploited at the application layer.
