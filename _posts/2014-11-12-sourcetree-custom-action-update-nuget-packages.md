---
layout: post
title: SourceTree Custom Action – Update NuGet Packages
date: 2014-11-12 18:51
categories: [programming]
tags: [git]
excerpt_separator: <!--more-->
---

Following on from my recent post on creating a <a href="{% post_url 2014-10-17-sourcetree-custom-action-open-fogbugz-case %}/">SourceTree custom action to open an FogBugz case</a>, I thought I'd share another custom action I have created. Again this custom action has been born out of the need to add some ease to a laborious process in our application life cycles.

<!--more-->

This new custom action allows you to update all or just a selected NuGet package for all solutions within a given repository, directly within the SourceTree client. This removes the need for a developer to open the solution (.sln) to just update the NuGet packages as previously once had to be done. It may only save 30-60 seconds but that will soon add up!

If you only used this once a day, that still adds up to 5 minutes a week and we all want to use our time efficiently right?

Now for the script!

{% highlight powershell %}
########################################################################
# Created By Ashley Poole - http://www.ashleypoole.co.uk               #
# Updates NuGet package(s) for solutions within a specified repository #
# SourceTree_UpdateNuGetPackage.ps1                                    #
########################################################################

Param(
    [string]
    $Repo,

    [string]
    $Package
)

# Get NuGet location and Solution files location
$NuGetExe = Get-ChildItem $Repo -Recurse -Filter 'NuGet.exe' | Select-Object -First 1
$NuGetExe = $NuGetExe.FullName
$SolutionFiles = Get-ChildItem $Repo -Recurse -Filter '*.sln'

# Only run package update if NuGet exists, else alert user
If ($NuGetExe)
{
    Foreach ($SolutionFile In $SolutionFiles)
    {
        # Getting solution file's full path
        $SolutionFile = $SolutionFile.FullName

        $ArgsUpdate = 'update ' + $SolutionFile

        # Checking if we are updating one or all packages
        If ($Package.ToUpper() -ne 'ALL')
        {
            $ArgsUpdate = $ArgsUpdate + ' -id ' + $Package
        }

        # Setting-up parameter for NuGet arguments
        $ArgsSelfUpdate = 'update -self'
        $ArgsRestore = 'restore ' + $SolutionFile

        # Update NuGet
        Start-Process $NuGetExe $ArgsSelfUpdate -NoNewWindow -Wait

        # Restore Packages
        Start-Process $NuGetExe $ArgsRestore -NoNewWindow -Wait

        # Update Package
        Start-Process $NuGetEx $ArgsUpdate -NoNewWindow -Wait
    }
}
else
{
    $wshell = New-Object -ComObject Wscript.Shell -ErrorAction Stop
    $wshell.Popup(&quot;NuGet.exe couldn't found. No Packages have been updated! :(&quot;,7,&quot;I'm Sorry!&quot;,0)
}
{% endhighlight %}

<em>(Sadly as you can see, the syntax highlighting isn't working correctly for this script so I recommend trying the GitHub link which can be found below in the note section.)</em>

In my example, I have saved the above script as C:SourceTree_UpdateNuGetPackage.ps1 though choose a location to suit your environment and preference. Once you have the script saved, open up the SourceTree client and navigate to the 'Tools' menu &gt; 'Options' &gt; 'Custom Actions' tab.

Note - I don't recommend saving files on the root of your C: (OS) drive but I choose that location as it allowed for a short file path for the example. I have also uploaded the script to my <a href="https://github.com/AshleyPoole/PowershellTools/tree/master/Scripts/SourceTree-UpdateNuGetPackages">GitHub PowerShell Tools repository</a> as well.

<strong>Updating All NuGet Packages</strong>
<ol>
	<li>Click the 'Add' button and enter 'Update All NuGet Packages' as the Menu Caption.</li>
	<li>Ensure 'Show Full Output' and 'Open in a separate window' are un-ticked.</li>
	<li>Enter 'Powershell.exe' for 'Script to run'.</li>
	<li>Enter '-noprofile -file C:SourceTree_UpdateNuGetPackages.ps1 $REPO ALL' for the 'parameters' field.</li>
</ol>
[caption id="attachment_1398" align="aligncenter" width="501"]<img class="wp-image-1398" src="http://www.ashleypoole.co.uk/wp-content/uploads/2014/11/Screen-Shot-2014-11-12-at-19.56.00.png" alt="SourceTree Custom Action Update NuGet Packages Setup" width="501" height="240" /> Update All NuGet Packages Setup[/caption]

<strong>Updating ABC NuGet Package</strong>

<em>Replace ABC with your package name.</em>
<ol>
	<li>Click the 'Add' button and enter 'Update ABC NuGet Package' as the Menu Caption.</li>
	<li>Ensure 'Show Full Output' and 'Open in a separate window' are un-ticked.</li>
	<li>Enter 'Powershell.exe' for 'Script to run'.</li>
	<li>Enter '-noprofile -file C:SourceTree_UpdateNuGetPackages.ps1 $REPO ABC' for the 'parameters' field.</li>
</ol>
&nbsp;

Now when you right click on any commit within a repository, you will see your new custom actions to either update all NuGet packages or just to update the one NuGet package we're referring to as ABC.

[caption id="attachment_1397" align="aligncenter" width="501"]<img class="wp-image-1397" src="http://www.ashleypoole.co.uk/wp-content/uploads/2014/11/Screen-Shot-2014-11-12-at-19.55.39.png" alt="SourceTree Custom Actions Menu Update NuGet Packages" width="562" height="89" /> SourceTree Custom Actions Menu - Update NuGet Packages options[/caption]


Hopefully this will come into use for others out there in the community too. Enjoy!
