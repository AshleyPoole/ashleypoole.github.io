---
layout: post
title: Transfer HostHeaders From Source To Destination Websites In IIS
date: 2013-12-16 19:53
categories: [devops, server-admin]
tags: [powershell, iis]
---
Well when you have multiple websites with multiple host headers, it would normally take a while to transfer host headers to a new group of websites. With the Powershell script below, this can achieved with very little set-up.

Note - Sadly the formatting (like indentation) has been lost in the post.

{% highlight powershell %}

## Written by Ashley Poole ##
## Description - Dynamically swaps host headers from the source website to destination website. The script will suffix the host heads on the resource website with '.reallocated.ps'. This script was written to easy the deployment of new websites during major releases ##
## Warning - This script will delete all bindings on the new website before copying others from the old website ##

import-module webadministration

# Create 2D array of source and destination website names objects
[array]$Websites = @(@("SourceWebsite1", "DestinationWebsite1"),@("SourceWebsite2","DestinationWebsite2"))

# Name suffix for old Host Headers
$SourceHostHeaderSuffix = ".reallocated.ps"

# Looping through website pairs and migrating host headers
foreach($Website in $Websites)
{
 # ** If we need to rollback host headers, Simply run this script again but swap $Website[0] and $Website[1] around **
 $SourceWebsiteName = $Website[0]
 $DestinationWebsiteName = $Website[1]

Write-Host "Evaluating Website: " $SourceWebsiteName "/" $DestinationWebsiteName -ForegroundColor Yellow

$Bindings = Get-WebBinding -Name $SourceWebsiteName | Select bindingInformation

 Try
 {
 # Remove all binding on the new website in preparation
 Get-WebBinding -Name $DestinationWebsiteName | Remove-WebBinding
 }
 Catch{}
 # Looping through each binding on from the current $website basis
 foreach($Binding in $Bindings)
 {
 $BindingBreakdown = $Binding.bindingInformation -split(":")

 # Breaking down items from the array to make them readable latter in script
 $BindingIPAddress = $BindingBreakdown[0]
 $BindingPort = $BindingBreakdown[1]
 $BindingOrginalHostHeader = $BindingBreakdown[2]
 $BindingSourceHostHeader = $BindingBreakdown[2]
 $BindingDestinationHostHeader = $BindingBreakdown[2]

Write-Host "Evaluating Binding:" $BindingOrginalHostHeader

# Checking if $BindingSourceHostHeader was already renamed (I.e contains value in $SourceHostHeaderSuffix). If so we need to reverse the process.
 If ($BindingSourceHostHeader -contains $SourceHostHeaderSuffix)
 {
 $BindingDestinationHostHeader = $BindingDestinationHostHeader -replace $SourceHostHeaderSuffix,""
 }
 else
 {
 $BindingSourceHostHeader = $BindingSourceHostHeader + $SourceHostHeaderSuffix
 }

$BindingInformation = $BindingIPAddress + ":" + $BindingPort + ":" + $BindingOrginalHostHeader

 # Renaming old binding
 Set-WebBinding -Name $SourceWebsiteName -BindingInformation $BindingInformation -PropertyName HostHeader -Value $BindingSourceHostHeader

# Add new binding
 New-WebBinding -Name $DestinationWebsiteName -Protocol http -IPAddress $BindingIPAddress -Port $BindingPort -HostHeader $BindingDestinationHostHeader

Write-Host "Completed Binding:" $BindingOrginalHostHeader
 }
 Write-Host "Completed Website:" $SourceWebsiteName "/" $DestinationWebsiteName -ForegroundColor Yellow
}

{% endhighlight %}
