---
layout: post
title: Check Metrics.NET Health Checks Using PowerShell
date: 2016-08-24 17:40
categories: [devops]
tags: [powershell]
---

This is a simple post detailing how you can use PowerShell to quickly check the health of an application that uses Metrics.NET. For anyone who doesn't know, Metrics.Net is a library that provides a way of instrumenting applications with custom metrics (timers, histograms, counters, etc) that can be reported in various ways and can provide insights on what is happening inside a running application.

More details on Metrics.NET can be found on their <a href="https://github.com/Recognos/Metrics.NET">GitHub repository</a> or <a href="https://github.com/Recognos/Metrics.NET/wiki">Wiki</a>.

The idea behind using PowerShell to query Metrics.NET health checks is that this script could be given to people in operations who can then monitor the help of the application without requiring knowledge of Metrics.NET or JSON. A message is also sent to the related developer's slack channel so they get a heads up that something isn't healthy and get in-front of the issue before it's reported :-)

To make this as re-usable as possible and cleaner to read, I created two PowerShell scripts, one containing the functions and the other for running the health checks.

Here's the script I've called '_MetricsDotNetHealthCheck.ps1' which contains the PowerShell functions.

{% highlight powershell %}
####################################################
##  Core health interface script for Metrics.Net  ##
##  Also includes Slack notifications             ##
##  Written by Ashley Poole                       ##
####################################################

Function Post-SlackMessage
{
	param([string]$channel, [string]$message, [string]$appChecked)

    Try
    {
        $SlackURL = "https://hooks.slack.com/services/[YOUR SLACK HOOK KEY GOES HERE]"

	    $payload = @{
					"channel" = $channel;
					"text" = $message;
				}

	    $payload = (ConvertTo-Json -Depth 3 -InputObject $payload)
	    Invoke-WebRequest -Method "POST" -Uri $SlackURL -Body $payload | Out-Null
    }
    Catch
    {
        Write-Host -ForegroundColor Red "Failed to notify $responsibleTeamSlackChannel slack channel of message: $message"
    }
}

Function Perform-MetricsDotNetHealthCheck
{
    param([string]$appName, [string]$appEnvironment, [string]$responsibleTeam, [string]$responsibleTeamSlackChannel, [string]$healthUrl)

    Try
    {
        $wc = New-Object System.Net.WebClient
        $health = $wc.DownloadString($healthUrl) | ConvertFrom-Json

        if($health.IsHealthy -ne "True")
        {
            Throw;
        }
    }
    Catch
    {
        $errorShort = "$appName in $appEnvironment is unhealthy or offline."
        # Here you could post a notification to Slack or other communication tools.
        # Post-SlackMessage $responsibleTeamSlackChannel "$errorShort Please take immediate action. Health check Uri: $healthUrl" $appName
        Write-Host -ForegroundColor Red "$errorShort The $responsibleTeam development team have been notified via the slack channel [$responsibleTeamSlackChannel]!"
        Return 1
    }

    Write-Host -ForegroundColor Green "$appName in $appEnvironment is healthy and online."
}
{% endhighlight %}

Finally, here's an example script to perform the health checks:
{% highlight powershell %}
# Import required health check functions
. .\_MetricsDotNetHealthCheck.ps1

# Set team information related to error messages and Slack
# Replace these with your own values
$responsibleTeam = "Backend Development Team"
$responsibleTeamSlackChannel = "BackendDev"

# Preform checks
# Replace these with your own values for application name, environment and Metrics.NET health check URI
Perform-MetricsDotNetHealthCheck "AnalyticsService" "PROD" $responsibleTeam $responsibleTeamSlackChannel "http://server01:5490/v1/health"
Perform-MetricsDotNetHealthCheck "CheckoutService" "STAGING" $responsibleTeam $responsibleTeamSlackChannel "http://server05:8285/v1/health"
{% endhighlight %}

Bingo, a quick and easy to use solution for gathering and checking the health of multiple applications which implement the Metrics.NET library. Enjoy!
