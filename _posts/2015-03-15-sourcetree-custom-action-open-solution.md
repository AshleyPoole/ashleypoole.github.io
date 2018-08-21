---
layout: post
title: SourceTree Custom Action – Open Solution
date: 2015-03-15 20:37
categories: [programming]
tags: [git]
excerpt_separator: <!--more-->
---
Day to day I find myself using Git natively as well as Atlassian's SourceTree Git client. Keen to ever ease my life with SourceTree I've previously written custom actions to <a title="sourcetree custom action open fogbugz case" href="{% post_url 2014-10-17-sourcetree-custom-action-open-fogbugz-case %}/" target="_blank">open the associated FogBugz case to a commit</a> as well as another custom action to <a title="sourcetree custom action update nuget packages" href="{% post_url 2014-11-12-sourcetree-custom-action-update-nuget-packages %}/" target="_blank">update all or certain NuGet packages for a repository</a>.

<!--more-->

Following on from this I have now created another custom action which I wanted to share that allows me to open solution files by simply right clicking on the repository and choosing my new custom action.
<h2>The Script</h2>
As you can see from the below script this is accomplished extremely easy, in fact in a single line of PowerShell which takes a single input parameter called 'Repo'.

If copying and pasting isn't your game then you can also find the script in my <a title="PowerShell Tools GitHub" href="https://github.com/AshleyPoole/PowershellTools/blob/master/Scripts/SourceTree-OpenSolution/SourceTree_OpenSolution.ps1" target="_blank">PowerShell Tools repository on GitHub</a> along with my previous creations.

{% highlight powershell %}
########################################################################
# Created By Ashley Poole - http://www.ashleypoole.co.uk               #
# Opens solution file(s) within a specified repository                 #
# SourceTree_OpenSolutions.ps1                                         #
########################################################################

Param(
    [string]
    $Repo
)

# Get and open solution files
Get-ChildItem $Repo -Recurse -Filter '*.sln' | Select { Start $_.FullName }

{% endhighlight %}

<h2>Setup</h2>
Setting the script as a custom action up in SourceTree is also extremely easy. Below is a screenshot of the options you need in order to get the custom action setup. Simply replace 'C:\scripts\...' with the location of where you have saved the script.

<img class="alignnone wp-image-5611 size-medium" src="{{ "/img/2015/03/open-solution-300x123.png" | prepend: site.assetsbaseurl }}" alt="SourceTree Create Custom Action" width="300" height="123" />
<h2>Usage</h2>
Once you have the custom action setup then it can be accessed by right clicking on the repository or a commit and choosing the new custom action called 'Open Solution' from within the 'Custom Actions' menu.

The solution files if found will then be opened in the default program associated with the file extension.

<img class="alignnone wp-image-5631 size-medium" src="{{ "/img/2015/03/right-click-open-solution-300x57.png" | prepend: site.assetsbaseurl }}" alt="Right click open solution custom action" width="300" height="57" />
