---
layout: post
title: Mastering the Art of CXPACKET and MAXDOP
date: 2014-01-09 19:55
categories: [general, databases]
tags: [webinar, sql-server]
---
Name : Mastering the Art of CXPACKET and MAXDOP
Author : Brent Ozar
Webinar Date : 09-01-2014

CXPACKET - Using parallel queries means there will be CXPACKET's as you are waiting on other items to finish. Normally you don't want more than 50%.

SQL Server doesn't evenly share work out before the schedulers. Queries go parallel int he following reasons:

<strong>1. When a query cost reaches a certain size</strong>

This is called the 'Cost threshold for parallelism' and is set on the server level. The default value is 5. It's unit of measure is the same as query costs. For modern servers this default value is too low.

On modern servers having this figure too low actually create's CXPACKET's as modern servers don't need to spilt queries up as much as they can cope with the load better.

<strong>2. Query would benefit from parallelism </strong>

Parallelism inhibitors - scalar user defined functions, mutil statements.... [SCREENSHOT]

<strong>3. Spreading it across multiple cores</strong>

MAXIMUM DEGREE OF PARALLELISM (MAXDOP). Default value is which means all cores. Set it up to the # of cores in one NUMA node in your server up to 8. NUMA is the number of cores in a single CPU for Intel. AMD may have multiple NUMA nodes in a single processor. Physical cores not hyper-threading cores.

You might also want to set MAXDROP slightly lower on a VM so a single query couldn't max out all the VM's CPU's. Although it depends on the application. Sharepoint suggests using MAXDROP of 1 though.

MS KB #2806535 - MAXDROP

brentozar.com/go/plancache

<strong>Why the defaults are bad</strong>

Having CXPACKET set low means queries will go parallel when they don't have too. Using all CPU's when they don't really need it.

Setting MAXDOP to 1 means all work (query) will go to one thread - very slow and doesn't share the load. This can affect and produce more SOS_SCHEDULER_YIELD waits for the query. This stat means the query could of finished faster with more CPU time, it however doesn't mean you need more CPU.

<strong>Moderate settings</strong>

Cost Threshold for Parallelism is rocket science. You need to look at the top CPU using queries in your plan cache so you can see their costs then set CXPACKET lower so the queries will go parallel.

Big queries should be tuned to run faster with CXPACKET and MAXDROP though not enough so it monopolises the entire servers CPU.

When you change either of these settings, SQL Server will empty the query cache. This means it has to be rebuilt as the queries come in.


Query Hints - You might think your being smart but your not, unless you can explain why the optimiser choose what it did.

Resource Governor - Use only after MAXDROP and Tuning, etc.
