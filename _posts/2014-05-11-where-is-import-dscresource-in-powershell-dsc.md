---
layout: post
title: Where is Import-DSCResource In Powershell DSC?
date: 2014-05-11 20:21
categories: [DevOps]
tags: [powershell, dsc]
---
I've recently been experimenting with Desired State Configuration (DSC) which I must say I love! I'm also very excited on the future of DSC and where it might be heading.

One evening though I decided to take my DSC script and move it over to a freshly created Windows Server 2012R2 server where I promptly noticed the Import-DSCResource didn't exist. I was using the Import-DSCResource to enable myself to use the extra DSC modules supplied in Microsoft's <a title="DSC Resource Kit" href="http://gallery.technet.microsoft.com/scriptcenter/DSC-Resource-Kit-All-c449312d" target="_blank">DSC Resource Kit</a>, namely the xWebAdminitration module. I was however stumped as my Import-DscResource command worked on my Windows 8.1 machine yet didn't on my Server 2012R2 instance. After a spending a while researching the topic I came across <a title="KB2883200" href="http://support.microsoft.com/kb/2883200/en-us" target="_blank">KB2883200</a> update by Microsoft which was classed as a general availability rollup.

Reading the summary of the rollup, it made no mention of Powershell or namely Import-DscResource, yet without this crucial update installed you are unable to use the Import-DscResource keyword. Below is the error I received when running a DSC script without the update installed.
<pre>PS C:Windowssystem32&gt; C:UsersAdministratorDesktopImport-DSCResource-Example.ps1
<span style="color: #ff0000;">At C:UsersAdministratorDesktopUntitled1.ps1:5 char:50
+     Import-DscResource -Module xWebAdministration
+                                                  ~
The Import-Module dynamic keyword requires names of the modules to import. The syntax of Import-Module dynamic keyword
is: "Import-Module [-Name] &lt;ModuleNames&gt;".
At C:UsersAdministratorDesktopUntitled1.ps1:4 char:1
+ {
+ ~
Missing closing '}' in statement block.
At C:UsersAdministratorDesktopUntitled1.ps1:107 char:1
+ }
+ ~
Unexpected token '}' in expression or statement.
    + CategoryInfo          : ParserError: (:) [], ParentContainsErrorRecordException
    + FullyQualifiedErrorId : ImportModuleNeedParams</span></pre>
Hopefully this will save someone else out there in the community the time I spent on researching and diagnosing this issue I experienced! Microsoft support article - <a title="KB Article" href="http://support.microsoft.com/kb/2883200/en-us" target="_blank">http://support.microsoft.com/kb/2883200/en-us</a> KB2883200 DownloadCentre link - <a title="KB2883200 Download Link" href="http://www.microsoft.com/en-us/download/details.aspx?id=40774" target="_blank">http://www.microsoft.com/en-us/download/details.aspx?id=40774</a>
