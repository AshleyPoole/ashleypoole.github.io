---
layout: post
title: 70-461 Chapter 01 - Foundations Of Querying
date: 2014-01-23 21:15
categories: [knowledge-base, databases]
tags: [sql-server, training]
---
<strong>Microsoft Exam ID :</strong> 70-461
<strong>Microsoft Exam Title :</strong> Querying Microsoft SQL Server 2012
<strong>Microsoft Exam Url :</strong> <a href="http://www.microsoft.com/learning/en-us/exam-70-461.aspx" target="_blank">http://www.microsoft.com/learning/en-us/exam-70-461.aspx</a>
<h2>Chapter One - Foundations Of Querying</h2>
<ul>
	<li>RDBMS - Relational Database Management System.</li>
	<li>SQL is a standard of both Intentional Organisation Standards (ISO) and American National Institute (ANSI) though there has been many major revisions over time including SQL:2008 and SQL:2011.</li>
	<li>Writing SQL in standard manor is considered best practice and makes it more portable.</li>
	<li>T-SQL supports two 'not equal to' expressions which are '&lt;&gt;' (standard) and '!=' (non-standard).</li>
	<li>CAST is the standard where as CONVERT is not. Both have different usage styles.</li>
	<li>CTE - Common Table Expression</li>
	<li>Standard SQL is based on the <em>relational database</em> model which is a mathematical model initially created by Edgar F. Codd in 1969. Relational in <em>relation database </em>does NOT have anything to do with the relationships between tables (foreign keys), it actually comes from the mathematical concept <em>relation</em>.</li>
	<li>A relation in the relational model is what SQL calls a table to represent a relation though some say this is not a very clear or successful attempt.</li>
	<li>SQL is based on the relation model though does differ.</li>
	<li>A relation (table) has a heading and body. The heading is a set of attributes (represented by columns) and an attribute is identified by name and type name. The body is a set of tuples (rows) and each tuple's heading is the heading of the relation.</li>
	<li>Most important principals regarding T-SQL stem from relational model's core foundations the set theory and predicate logic.</li>
	<li>Set theory - interact with the set as a <strong>whole</strong> not on a element by element basis as well as the set containing no duplicates. I.e {a,b,c} is equal to {a,b,b,c,c}. Another aspect that isn't defined by implied is that there aren't any relevance to the order of the elements in a set.</li>
	<li>Predicate logic - an predicate is an expression when attributed to an object, i.e 'Price less than £10.00'. You can evaluate an predicate to get a true or false proposition. Predicates are used to enforce data integrity as well as filtering data.</li>
</ul>
<h4>Using T-SQL In A Relational Way</h4>
<ul>
	<li>In T-SQL you should try to avoid using iterative constructs like cursors and loops that iterate through rows one by one.</li>
	<li>T-SQL doesn't always enforce that a set doesn't have duplicates, i.e you can have a table without a key and duplicate rows. You should though enforce uniqueness in your tables using a primary key or unique constraint for example. Even though a table doesn't allow duplicate rows a query against that table can still return duplicates in the superset (not a 'set' as that doesn't contain duplicates). I.e selecting just the city column may return duplicates.</li>
	<li>T-SQL is actually more based on multiset theory than on set theory. A multiset can also be called a superset and does allow duplicates though this isn't based on the relational theory. The DISTINCT clause is used to remove duplicates to make the data returned a set rather than a superset.</li>
	<li>A 'set' doesn't state an relevance to the order of the elements and even though you may understand the physical representation of the data you shouldn't assume any order.</li>
	<li>When using an ORDER BY the results aren't relational and is what standard SQL calls a <em>cursor</em>.</li>
	<li>Columns are presented in the order as they are stored in the table definition.</li>
	<li>Resulting columns in T-SQL don't have to have an assigned name on the target column though this means it breaks away from the relation model where all attributes must have names. These attributes must also be unique.</li>
	<li>As well as a predicate evaluating to True or False, Codd wanted to reflect two different missing values. though being as T-SQL is based on SQL there is only one general purpose mark called NULL.</li>
	<li>It's important to understand what happens when NULL's are involved in the data your querying like sorting, filtering, grouping, joining and intersecting.</li>
</ul>
<h4>Using Correct Terminology</h4>
<ul>
	<li>A column and row are not an field and record. Tables are logical and have logical rows and columns.</li>
	<li>"Null Value" is wrong. NULL is not a value, it is a term to describe a missing value.</li>
</ul>

<h4>Logical Query Processing</h4>
<ul>
	<li>Logical is the conceptual interpretation of the query which explains what the correct result is. Physical side of query processing is the done by the database engine which produces the results defined by the logical query processing.</li>
	<li>The database engine can apply optimisation which means it can rearrange steps from the logical query or remove steps but only as the end result remains the same as defined by the logical query.</li>
	<li>T-SQL is a declarative english like language. You define what you want as opposed to imperative languages that also define how to achieve the result. T-SQL is written to define the 'What' part of the query and the database engine is responsible to figure out the physical how part of the operation.</li>
	<li>Don't draw any performance related conclusions regarding logical query processing. It's important to understand what's happening under the hood during the optimisation stage of the database engine.</li>
	<li>Originally SQL was called SEQUEL which stood for 'Structured English Query Language' though was renamed after a trademark dispute that now stands for 'Structured Query Language'.</li>
</ul>
<h6>Logical Query Processing Phases</h6>
<ol>
	<li>FROM</li>
	<li>WHERE</li>
	<li>GROUP BY</li>
	<li>HAVING</li>
	<li>SELECT</li>
	<li>ORDER BY</li>
</ol>
Keyed In Order
<ol>
	<li>SELECT</li>
	<li>FROM</li>
	<li>WHERE</li>
	<li>GROUP BY</li>
	<li>HAVING</li>
	<li>ORDER BY</li>
</ol>
<ul>
	<li>Output from each logical steps is returned as a virtual table and is considered the input from the next phase.</li>
	<li>Using Order By means the results are relational.</li>
	<li>EXAM TIP - Rows which the predicate evaluates to false or to an unknown state as not returned.</li>
	<li>Column aliases cannot be used in the WHERE statement as the WHERE statement is evaluated before the SELECT statement. A common mistake by someone who doesn't understand logical processing. Aliases can however be used in latter logical processing stages like ORDER BY though cannot be used in the same logical processing phrase they are declared in. I.e creating an alias in the SELECT, cannot then also be used else where in the SELECT statement.</li>
	<li>HAVING filters results based on a predicate after the results have been grouped compared to WHERE which filters results on an per row basis.</li>
	<li>As SQL is more based on multiset theory it's your responsibility to remove duplicates using the DISTINCT clause.</li>
	<li>As SELECT is processed by the ORDER BY, the SELECT output is considered to be relation still.</li>
	<li>The result of ORDER BY is what standard SQL calls a cursor though the term cursor here is used conceptual. T-SQL also supports an object called a cursor that is defined on results of a query and allows rows to be fetched one by one in a specified order.</li>
</ul>
