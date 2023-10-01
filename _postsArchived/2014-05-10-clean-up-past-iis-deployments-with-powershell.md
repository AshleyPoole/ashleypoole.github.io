---
layout: post
title: Clean-up Past IIS Deployments With Powershell
date: 2014-05-10 15:45
categories: [devops, server-admin]
---
Faced with an issue of rapidly running out of free disk space on our IIS web servers, one evening I set about creating a Powershell script to automate the process of removing unused website deployments from our web server farm. Previously it had been a manual process to clean-up these now unused deployments which were left behind by our versioned deployment process. As you can imagine this task was a time consuming process that previously had no automated solution that was smart enough to detect which builds were no longer needed.

The outcome to resolving this issue was the below script. If executing the script without the '-delete True' parameter the script will run in a preview style mode and will list builds it would have removed. This is great as you can do a manual check before allowing the script to delete actual files.

Note - This script assumes you websites folder structure will look like this [WebsiteName][WebsiteBuild]Content and that the physical path in IIS for the website is set to the 'content' folder.

{% highlight powershell %}
<#
Written By: Ashley Poole
Date: 21/04/2014
Description: Designed to removed old builds from the web server
#>
param($delete='False')

Import-Module WebAdministration -ErrorAction Stop

# Pulling list of websites from IIS where it's not the default website and the physical path ends in content. The word content indicates a powerdeployed website.
$websites = Get-Website | where {$_.Name -ne  "Default Web Site " -and $_.PhysicalPath -like  "*content "}

foreach ($website in $websites)
{
	$parentFolderPath = (Get-Item $website.physicalpath).parent.parent.fullname
	$currentFolderName = (Get-Item $website.physicalpath).parent.Name

	Write-Host  "***** EVALUATING WEBSITE : " $website.name  "***** " -ForegroundColor Cyan
	Write-Host  "Parent Folder Path: " $parentFolderPath -ForegroundColor Yellow
	Write-Host  "Live Folder Name: " $currentFolderName -ForegroundColor Yellow

	# Sorting by LastWriteTIme rather than Name due to the same build could be deployed mutiple times caused by a rollback
	$websiteFolders = Get-ChildItem $parentFolderPath | where {$_.Name -ne $currentFolderName  -and $_.Name -notlike  "*.config "} | sort -Property LastWriteTime

	$websiteFoldersCount = $websiteFolders.Length -1
	$count = 2

	foreach ($folder in $websiteFolders)
	{
		# Only run if we have 3 or more folders lefts
		if ($count -le $websiteFoldersCount)
		{
			# Final check to ensure the folder being deleted isn't the live folder
			if ($folder -ne $website.physicalpath)
			{
				if ($delete -eq 'true')
				{
					Write-Host  "Deleting : " $parentFolderPath$folder
					# Removing folder recursively
					Remove-Item $parentFolderPath$folder -Force -Recurse
				}
				else
				{
					Write-Host  "Deleting (PREVIEW): " $parentFolderPath$folder
				}
			}
		}

		$count++
	}
}
{% endhighlight %}

<!--more-->

Alternatively this script is available on my <a title="Ashley Poole GitHub" href="https://github.com/AshleyPoole" target="_blank">GitHub</a> account - <a title="Cleanup IIS Website Deployments" href="https://github.com/AshleyPoole/PowershellTools/blob/master/Scripts/CleanUpIISWebsiteDeployments/CleanupIISWebsiteDeployments.ps1" target="_blank">https://github.com/AshleyPoole/PowershellTools/blob/master/Scripts/CleanUpIISWebsiteDeployments/CleanupIISWebsiteDeployments.ps1</a>
