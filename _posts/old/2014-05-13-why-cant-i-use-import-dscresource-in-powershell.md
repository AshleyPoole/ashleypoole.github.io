---
layout: post
title: Why Can't I Use Import-DscResource In Powershell?
date: 2014-05-13 19:09
categories: [DevOps]
tags: [powershell, dsc]
---
So you've been playing about with Powershell for a while and decided to take the plunge with trying out Desired State Configuration (DSC) yet you've hit a snag with Import-DscResource. You've of course read my previous article titled "<a title="Where Is Import-DscResource In Powershell DSC" href="http://www.ashleypoole.co.uk/2014/where-is-import-dscresource-in-powershell-dsc/" target="_blank">Where is Import-DSCResource In Powershell DSC?</a>" but still can't seem to get Import-DscResource to be recognised by Windows Powershell. You next move was likely to try running something like 'Get-Help *DscResource*' yet your still left sitting there scratching your head, as while some results were returned by Get-Help, no import method was listed for DSC resources. Nice try though!

The reason for this is because Import-DscResource is a dynamic keyboard and is only available within a DSC script configuration block. Some people confuse Import-DscResource with a cmdlet due to it's syntax, though it isn't a cmdlet and such behaves differently. Trying to use Import-DscResource as a cmdlet will result in the below error:

PS C:UsersAdministrator&gt; Import-DscConfiguration -Module xWebAdministration
<span style="color: #ff0000;">Import-DscConfiguration : The term 'Import-DscConfiguration' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.</span>
<span style="color: #ff0000;"> At line:1 char:1</span>
<span style="color: #ff0000;"> + Import-DscConfiguration -Module xWebAdministration</span>
<span style="color: #ff0000;"> + ~~~~~~~~~~~~~~~~~~~~~~~</span>
<span style="color: #ff0000;">   + CategoryInfo : ObjectNotFound: (Import-DscConfiguration:String) [], CommandNotFoundException</span>
<span style="color: #ff0000;">   + FullyQualifiedErrorId : CommandNotFoundException</span>
