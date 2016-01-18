---
layout: post
title: T-SQL Proc Template
date: 2013-08-19 18:00
categories: [databases, knowledge-base]
tags: [sql-server]
excerpt: Example stored procedure creation script.
---
Example stored procedure creation script.

{% highlight sql %}
IF OBJECT_ID ( 'dbo.my_proc', 'P' ) IS NOT NULL
DROP PROCEDURE dbo.my_proc
GO

CREATE PROCEDURE [dbo].[my_proc]
    @startDate DateTime,
    @endDate DateTime,
AS
BEGIN
    SELECT
        myID,
        myName
    FROM myTableName
    WHERE myStartDate &gt;= @startDate
    ORDER BY @endDate
END
GO
{% endhighlight %}
