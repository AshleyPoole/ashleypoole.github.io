---
layout: post
title: RavenDB Notes
date: 2014-06-22 20:40
categories: [databases, knowledge-base]
tags: [notes]
---
A few notes from my personal exploration of RavenDB so far.
<h2>Under-The-Hood</h2>
<ul>
	<li>Document database part of the NoSQL database offerings.</li>
	<li>Is transactional.</li>
	<li>Open source.</li>
	<li>Written in .NET and required .NET Framework 4.0.</li>
	<li>Data is stored as a schema-less JSON document.</li>
	<li>From .NET it can be queried using LINQ.</li>
	<li>Indexes are automatically created based on usage though can be specified by the consumer.</li>
	<li>Two storage engines are supported called Esent and Munin though only Esent is supported for a production environment.</li>
	<li>A single data entry is called a 'Document'.</li>
	<li>Each document has MetaData attached to it by default which contains information that is used internally by RavenDB.</li>
	<li>A 'Collection' is a set of documents sharing the same RavenDB entity type. This is only a logical/ virtual construct.</li>
	<li>Each document has it's own unique global ID. Reusing the same ID will overwrite the existing document.</li>
	<li>'RavenDB Management Studio' is the RavenDB equivalent to SSMS.
Uses MapReduce in the background and indexes are running asynchronously.</li>
	<li>RavenDB 3.0 should be able to run on Mono.</li>
</ul>
<h2>Safe By Default</h2>
<ul>
	<li>Result set limits (Unbounded result sets)
<ul>
	<li>Failing to specify the page size on the client (using Take() method) will default to a 128 page size.</li>
	<li>On the server side, page sizes greater than 1024 will be reduced to 1024.</li>
	<li>The server side limit can be adjusted using Rave/MaxPageSize though this action isn't recommended.</li>
</ul>
</li>
	<li>Excessive querying (Unbounded number of requests)
<ul>
	<li>Number of requests from each clients session is limited to 30 by default.</li>
	<li>Any queries over this limit will throw an exception.</li>
	<li>This limit can be overwritten on by either a global limit or per client session.</li>
</ul>
</li>
</ul>

<h2>Consumption</h2>
<ul>
	<li>RavenDB can be consumed / connected to using one of two methods.
<ul>
	<li>.NET Client API - Recommend were ever possible</li>
	<li>HTTP API</li>
</ul>
</li>
	<li><em>TO BE FLESHED OUT</em></li>
</ul>
<h2>Index Administration</h2>
<ul>
	<li>Resetting an index is usually needed if the error quota has been reached and the index has been taken offline. This can be achieved in three ways:
<ul>
	<li>.NET API call - <code class="csharp plain" style="color: black !important;">store.DatabaseCommands.ResetIndex(</code><code class="csharp string" style="color: #db2e62 !important;">"IndexName"</code><code class="csharp plain" style="color: black !important;">);</code>.</li>
	<li>HTTP API call - <span style="color: #000000;">curl -X RESET </span><a style="color: #9b002b;" href="http://localhost:8080/databases/DB1/indexes/indexName">http://localhost:8080/databases/DB1/indexes/indexName</a>.</li>
	<li>Within the Studio, right click index and select 'Reset index' option.</li>
</ul>
</li>
	<li>An index can be deleted using one of these three methods:
<ul>
	<li>.NET API Call - <code class="csharp plain" style="color: black !important;">store.DatabaseCommands.DeleteIndex(</code><code class="csharp string" style="color: #db2e62 !important;">"IndexName"</code><code class="csharp plain" style="color: black !important;">);</code></li>
	<li>HTTP API call - <span style="color: #000000;">curl -X DELETE </span><a style="color: #9b002b;" href="http://localhost:8080/databases/DB1/indexes/indexName">http://localhost:8080/databases/DB1/indexes/indexName</a>.</li>
	<li>Studio. Same instructions as resetting an index.</li>
</ul>
</li>
	<li>Index locking can be used to prevent tampering. The three index locking modes are:
<ul>
	<li>Unlock</li>
	<li>LockedIgnore</li>
	<li>LockedError</li>
</ul>
</li>
	<li>Indexing priorities can be set.</li>
	<li>Auto indexes can be persisted to disk.</li>
</ul>
<h2>Backup And Restore</h2>
<ul>
	<li>A backup can be triggered using two methods:
<ul>
	<li><span style="color: #000000;">The url must be that of the database. Point it at the root will backup the system database - Raven.Backup.exe --url=</span><a style="color: #9b002b;" href="http://localhost:8080/">http://localhost:8080/</a><span style="color: #000000;"> --dest=C:TempFoo</span></li>
	<li><code class="json plain" style="color: black !important;">curl -X POST <a style="font-style: normal !important; color: #9b002b;" href="http://localhost/">http://localhost</a></code><code class="json keyword" style="font-weight: bold !important; color: blue !important;">:</code><code class="json plain" style="color: black !important;">8080/admin/backup -d </code><code class="json string" style="color: #db2e62 !important;">"{ 'BackupLocation': 'C:\Backups\2010-05-06' }"</code></li>
</ul>
</li>
	<li>To restore a database you need to use the Raven.Server.Exe - <span style="color: #000000;">Raven.Server.exe -src [backup location] -dest [restore location] -restore</span></li>
	<li>If restoring to an existing data directory, you will need to first remove all data files.</li>
	<li>Backups are async and restores are fully sync.</li>
</ul>
