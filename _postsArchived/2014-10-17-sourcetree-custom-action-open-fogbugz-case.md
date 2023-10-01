---
layout: post
title: SourceTree Custom Action - Open FogBugz Case
date: 2014-10-17 22:25
categories: [programming]
tags: [git]
excerpt_separator: <!--more-->
---
<p>Today I was asked by one of our developers for help in creating a PowerShell script that would be used within the SourceTree client which of course piqued my interest, especially with all my recent work with PowerShell. Therefore this evening I cracked up my Windows 8.1 development VM and created the script which your see below based on the requirements.</p>

<!--more-->

<p>The requirements were that the script should be able to search the specified commit's message for the 'BugzID' (FogBugz case ID) and open the relevant FogBugz case in the browser. For reference, we use the BugzID property to tie commits up to their cases for auditing purposes.</p>
The script is written to be used with the SourceTree client which has a feature called <a href="http://blog.sourcetreeapp.com/2012/02/08/custom-actions-more-power-to-you/" target="_blank">Custom Actions</a> that allows you to extend the range of functionality available within the GUI with your own commands or scripts. If your unsure about how to add Custom Actions in SourceTree or want to know more, see their blog article <a href="http://blog.sourcetreeapp.com/2012/02/08/custom-actions-more-power-to-you/" target="_blank">here</a> on the topic. You can also use the script by simply running it and passing it in a folder path to a GIT repository followed by the commit SHA.

Now to the fun bit, the script!

{% highlight powershell %}
##################################################################
# Created By Ashley Poole - http://www.ashleypoole.co.uk         #
# Opens FogBugz case (BugzID) if specified in the commit message #
# SourceTree_OpenFogBugzCase.ps1                                 #
##################################################################

Param(
    [string]
    $Repo,

    [string]
    $SHA
)

$CommitMessage = git -C $Repo log --format=%B -n 1 $SHA
$BugzID = $CommitMessage | Select-String -pattern &quot;BugzIDs*:s*d*&quot; | select-object -expand Matches
$BugzID = $BugzID -replace &quot;[^0-9]&quot;

If ($BugzID.length -ne 0)
{
    start ('http://fogbugz/?' + $BugzID)
} else {
    $wshell = New-Object -ComObject Wscript.Shell -ErrorAction Stop
    $wshell.Popup(&quot;No BugzID could be found for this commit :(&quot;,7,&quot;I'm Sorry!&quot;,0)
}

{% endhighlight %}

The script gets the commit message relating to the SHA for the specified repository. It then parses the commit message looking for a string like "BugzID: 12345" and stores the numeric characters in $BugzID. This is then appended to the FogBugz Url in the hope it's a valid case ID and is opened using the users default browser.

To use the script you just need to save it (i.e SourceTree_OpenFogBugzCase.ps1) and add it as a custom action. Here is an example screenshot of the custom action entry if you would have saved the script as C:TempSourceTree_OpenFogBugzCase.ps1.

<img class="aligncenter size-full wp-image-1291" src="http://www.ashleypoole.co.uk/wp-content/uploads/2014/10/sourcetree-customactions.png" alt="SourceTree Custon Action" width="509" height="239" />

&nbsp;

Enjoy!

P.S - If you keep getting an error about the BugID not being found, this might be caused by GIT.exe not being in your environments path.

&nbsp;
