---
layout: post
title: New Relic APM Spring Clean
date: 2019-03-04 19:20
categories: [devops]
tags: [newrelic]
---
Having a spring clean of New Relic APM might be a little tedious but luckily New Relic provide a feature rich API which makes this job a breeze.

Below is an Powershell script that will loop through all the non-reporting applications from your New Relic account based on the API key, and issue a delete request to their API for each of those applications.

{% highlight powershell %}
Param(
  [string]$apiKey
)

$appResponse = Invoke-RestMethod -Uri "https://api.newrelic.com/v2/applications.json" -Method Get -Headers @{'X-Api-Key'="$apiKey"}

$nonReportingApps = $AppResponse.applications | Where-Object {$_.reporting -eq $false}

Write-Host "Found $($nonReportingApps.Count) non reporting applications in New Relic"

foreach ($nonReportingApp in $nonReportingApps)
{
    Write-Host "Deleting New Relic application with Id $($nonReportingApp.id) ($($nonReportingApp.name))"

    try {
        Invoke-RestMethod -Uri "https://api.newrelic.com/v2/applications/$($nonReportingApp.id).json" -Method Delete -Headers @{'X-Api-Key'="$apiKey"} | Out-Null
    } catch {
        Write-Error "Failed to delete application, status code $($_.Exception.Response.StatusCode.value__)"
    }
}
{% endhighlight %}

Enjoy your New Relic spring clean!