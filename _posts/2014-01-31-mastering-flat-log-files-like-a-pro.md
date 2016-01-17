---
layout: post
title: Mastering flat log files like a Pro!
date: 2014-01-31 21:25
author: ashley@ashleypoole.co.uk
comments: true
categories: [devops, programming]
---
Debugging software isn't all just about the glamours draw of the IDE or code base. Sometimes you must first get your hands dirty in a mountain of logs to actually narrow down the issue reported.

<span style="font-size: 1rem; line-height: 1.5;">It goes without saying with the extremely cheap price of computing resources these days including storage, most applications of any size will be storing logs </span>of all kinds. These are your friends in your hour of need, when you have that mission critical system down or that bug that keeps rearing it's ugly head.

In my time, I've worked with a number of systems each with their own logging styles, though these can be broken down into two main groups. Group one that log entries in a database or log management tool and those that log to flat log files. Debates happen every day in the world of IT and Development whether these log entries should be written to flat log files or databases, with both having their own advantages and disadvantages.

Going into the ins and outs of why database is better than flat file or vice versa is out the scope of my post though I will say I personally prefer application / auditing log entries where appropriate to be sorted within a database. For some applications, storing log entries in a database would not be viable due to the overhead of hitting the database each time.

<h3>My playing field before...</h3>
On an average day, I have the need to open between 1 and about 20 flat log files when looking into an issue and searching across them for entries matching my criteria (IP address, email address, etc). My tool of choice had up to this point been my favourite text editor Notepad++ using it's 'Find all' feature across all open files.

<h3>What changed all this?</h3>
Using Notepad++ had to a certain extent being working fairly well expect when I tried to open a large number and large file size group of logs which it then struggled with. When I developed the need to consolidate flat log files from multiple applications and multiple nodes I knew Notepad++ just wouldn't cut it and I wouldn't expect a text or coding editor to either.

After carrying out some research I considered three options:
<ol>
	<li>Import logs into a database and querying them with SQL</li>
	<li>Use <a title="Log Parser by Microsoft" href="http://www.microsoft.com/en-us/download/details.aspx?id=24659" target="_blank">Log Parser</a> by Microsoft</li>
	<li>Use log management tool</li>
</ol>
Straight away I ruled out #3 due to it would require a new software installation of some kind and I needed something for the short term which would give me the shortest report to resolution time.

Next I evaluated my preferred idea of bulk importing my logs into a SQL Server database and using T-SQL to query them. This idea though would of too needed me to sanitise the logs first to remove headers, etc and would also not fit in with my current short term goal. Regrettably I shelved this idea although I do plan on revisiting this in the future as time allows.

So that leaves....

<h3>Log Parser</h3>
<a title="Log Parser" href="http://www.microsoft.com/en-us/download/details.aspx?id=24659" target="_blank">Log Parser</a> is a versatile tool that provides query access to a number of different sources including log, xml and csv files as well key data sources found on Windows operating systems like the event log and registry. The tool was last published in 2005 yet still remains highly useable.

I had heard about Log Parser before though up to this point hadn't had a need to research it much further until today. While I was researching Log Parser I also came across another tool written by the Microsoft Exchange team called Log Parser Studio which is a graphical user interface for the Log Parser tool.

I installed both Log Parser and Log Parser Studio though I actually wrote my own query statements due to they didn't match any of the default templates in Log Parser Studio already. Within minutes of writing a query to pull all entries matching an IP address, I had searched over 125 log files  over 1.25GB in size.

Log Parser uses a querying syntax very similar to SQL which means you can pick this tool up and run with it in a fairly short amount of time. Admittedly, the search was much slower than if I had imported the logs into a carefully indexed database though for an ad-hoc search this tool is perfect if you don't want to craft an import routine unless you have pretty well formatted log files in-which you can then just use BULK INSERT within T-SQL.

<a href="{{ "/img/2014/01/LogParser.png" | prepend: site.assetsbaseurl }}""><img class="aligncenter size-full wp-image-1022" src="http://www.ashleypoole.co.uk/wp-content/uploads/2014/01/LogParser.png" alt="LogParserImage" width="624" height="395" /></a>

For more information on usage, I recommend the below links. Enjoy!

http://technet.microsoft.com/en-us/library/ee692659.aspx
http://blogs.technet.com/b/karywa/archive/2013/06/05/log-parser-studio-write-your-first-query-in-less-than-30-seconds.aspx
